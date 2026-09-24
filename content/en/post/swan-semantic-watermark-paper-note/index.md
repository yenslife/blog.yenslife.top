+++
date = '2026-09-22T11:24:16+08:00'
draft = false
title = 'SWAN Paper Notes: Semantic Watermarking with Abstract Meaning Representation'
description = 'Notes on the ACL 2026 paper SWAN: embedding watermark signals into AMR semantic structure and guiding LLM generation with AMR templates to resist paraphrasing attacks'
image = 'fig2-overview.png'
categories = ['paper-notes']
tags = ['LLM', 'Watermark', 'AMR', 'Semantic Watermark', 'Seminar']
keywords = ['SWAN', 'Semantic Watermarking', 'AMR', 'Abstract Meaning Representation', 'LLM watermark', 'paraphrase attack', 'watermarking', 'paper notes']
math = true
+++

# SWAN: Semantic Watermarking with Abstract Meaning Representation

- ACL 2026 main
- Embeds the watermark signal in the semantic representation, so compared with token-level watermarks this approach is more robust to paraphrasing
- Produces text output carrying a semantic watermark signal by prompting the LLM with an AMR template

> **Disclaimer**: These are my personal notes written while preparing a seminar presentation. They exist mainly to help me understand the paper and are not a rigorous paper review. There may be misunderstandings or imprecise statements. If you spot a mistake, feel free to point it out in the comments. I recommend reading this alongside the [original paper](https://aclanthology.org/2026.acl-long.1681/).

Existing watermarking methods such as SynthID-Text (or KGW-like token-level methods) and embedding-level methods are all vulnerable to paraphrasing attacks, where another LLM rewrites the watermarked text to wash out the watermark signal. This is not the first paper to embed a watermark signal using semantics, though. A problem with existing methods is that the rewritten text may have its embedding shifted, which still hurts detection performance.

Actually, I have not read any embedding-level watermark papers, so I just asked ChatGPT. The gist is that the sentence embedding is used as the watermark signal; you can think of it as a different sampling granularity, not token by token.

This paper uses AMR to represent sentences, but can it represent special formats like code or JSON? I wonder whether they considered this problem. And what about different languages? Let's wait and see (TODO: come back and answer this after finishing the paper)

## Background

I only record the parts I had not seen before
### Abstract Meaning Representation (AMR)

AMR is a way of representing an abstracted sentence as a Graph, with the goal of abstracting away syntactic and lexical differences between sentences. An example makes it easier to understand. The following four sentences are constructed completely differently, yet their AMR is the same:
- The boy desires the girl to believe him.
- The boy desires to be believed by the girl.
- The boy has a desire to be believed by the girl.
- The boy’s desire is for the girl to believe him.
You can think of all of them as: "boy wants (girl believes boy)"

$$\begin{align*}
1 \quad & (w \ / \ \text{want-01} \\
2 \quad & \quad :\text{ARG0} \ (b \ / \ \text{boy}) \\
3 \quad & \quad :\text{ARG1} \ (b2 \ / \ \text{believe-01} \\
4 \quad & \qquad\qquad :\text{ARG0} \ (g \ / \ \text{girl}) \\
5 \quad & \qquad\qquad :\text{ARG1} \ b))
\end{align*}$$

The AMR representation is shown in figure 1, where nodes are concepts and edges are semantic relations:
![figure 1 AMR graph visualization](fig1-amr-graph.png)


### Text Watermarking

Text watermarking can be divided into the following three categories by granularity
- token-level: apply a slight bias to the sampling distribution during decoding to embed a statistical signal. The classic KGW and the more advanced [SynthID-Text](/en/post/synthid-text-paper-note/) both belong here. These methods are helpless against paraphrasing and surface-level text edits.
- sentence-level: to improve paraphrase robustness, the watermark signal is encoded in semantic space. Similar methods include SemStamp and k-SemStamp (I definitely need to look these up later).
- paragraph-level: roughly two kinds, "embedding the watermark at the paragraph level" and "injecting the watermark in a black-box manner after the text is generated (post-hoc)". PostMark is one example: it first encodes the semantics of the paragraph, selects specific watermark words from an embedding table, then has an instruction-following LLM weave them into the original text. This approach is black-box because it needs neither the Target Model's weights nor its sampling algorithm

And SWAN in this paper? The authors say that unlike the three above, it embeds the watermark directly into the semantic structure of the sentence.

## Overview of the Framework

![figure2 Overview](fig2-overview.png)

### AMR Bank Creation

The authors use the MASSIVE-AMR dataset, which has about 84000 AMR graphs covering 1685 types of data query utterances

The raw AMRs are then abstracted into template AMRs, for example by replacing nouns with generic placeholders; Alice might become NE (Named Entity). There are two advantages to using template AMRs
- Generality: a small number of template AMRs can map to a large number of different sentences
- Robustness: replacing entity names reduces the chance that lexical edits destroy the watermark

So how did they select them? To build the AMR bank, the authors go through all of MASSIVE-AMR and keep only the AMR graphs satisfying both of the following conditions:
- Frequency between 3 and 20 occurrences (excluding ones that are too unusual and ones that are too formulaic)
- At least 3 concept nodes (they do not want the AMR to be too simple)

The Secret Bank $\mathcal{B}$ here is equivalent to the concept of the watermark's Key, used in both Injection and Detection.
### Watermark Injection

Before generating a sentence, a template $g$ is drawn from the AMR bank, and the LLM is given two inputs:
- The Context accumulated from previously generated sentences, to maintain discourse coherence.
- The template $g$.

The LLM is then instructed to generate text that matches the semantic structure of that template. The whole Prompt looks like this (from appendix A.1)
```plain
AMR (Abstract Meaning Representation) is a graph-based representation of a sentence's meaning. Each node is a concept and edges represent semantic roles or relationships.
Below are some examples of template AMRs and corresponding sentences:
{example_text}

In the provided AMR, there are placeholders:
- "NE" for named entities (e.g., "Alice", "France", "Google").
- "N" for generic nouns (e.g., "a device", "an object").
- "X" for unspecified concepts (e.g., "something", "an idea").

Instructions:
- Do not write "NE", "N", or "X" literally. Instead, replace them with appropriate English words to form a natural, meaningful sentence.
- Ensure the generated sentence aligns with both the AMR structure and the given context.
- Do not produce multiple sentences or lists.
- Produce exactly one coherent sentence.

AMR:
{chosen_template}
Context: {context}
Please output only that one sentence.
```

The LLM's output is then parsed back into an AMR Graph $\hat{g}$ by an AMR Parser. For every generated graph $\hat{g}$ we compute S2MATCH: $S2MATCH(\hat{g},g)$, its similarity to the AMR template $g$. The authors describe S2MATCH as a lightweight metric that takes node-level and edge-level F1 scores into account and outputs a value between 0 and 1. That is, the sentence is only accepted when $S2MATCH(\hat{g},g)>\theta_{accept}$; otherwise it is regenerated.

Because the output is influenced by the context, the LLM does not always match the template so neatly, so they set a small upper limit on the number of resampling attempts. If the limit is reached without success, the current template is abandoned and another one is drawn from the bank for generation. (Adaptive Target Switching)

The full Algorithm is shown below:

![Algorithm 1](algorithm1-injection.png)

### Watermark Detection

SWAN detects a piece of text in three steps
1. Use an off-the-shelf AMR Parser to convert each sentence into an AMR Graph
2. For the $\hat{g}$ parsed from a given sentence, compare it against all templates in the secret bank (S2MATCH) and take the maximum. If the highest similarity exceeds the threshold, mark it as a green sentence.
3. A paragraph may have n sentences, so a one-proportion z-test is also needed. If k sentences are marked as green sentences, compute $z = \frac{k-\lambda n}{\sqrt{n \lambda (1- \lambda)}}$, where $\lambda$ is the expected hit-rate.

The null hypothesis $H_0$ here is "this text was written without knowledge of the secret bank"

Below is the Paragraph-Level detection algorithm
![algorithm 2](algorithm2-detection.png)

## Experiments

The experiments follow the SemStamp baseline setup on the REALNEWS subset of the C4 corpus, a Google open-source news index dataset composed of professionally written news articles. The first 250 sentences of the subset are taken as the evaluation set.

Then there is the AMR bank size, which must be set carefully because it is used in both injection and detection, especially detection, which has to scan through all templates. Here they set $|\mathcal{B}| = 50$.

All watermark injection models use `DeepSeek-R1-Distill-Qwen-14B`. The algorithm tries at most $M=5$ times and switches through at most $T=10$ different AMR templates per sentence. By comparison, SemStamp allows up to 100 attempts per sentence, whereas here it is only 50.

For detection, they use `parse_xfm_bar_large`, which is a model, and only then run S2MATCH.

For paraphrasing attacks, they use Pegasus (dedicated to paraphrasing), Parrot (a T5-based model, meaning it is good at swapping words), and Claude 3.7 Sonnet to create the attacks. For text quality, Claude 3.7 Sonnet is used in a Zero-shot manner to evaluate Coherence, Fluency, and Diversity, i.e. LLM-as-a-Judge.

The evaluation metrics can be divided into two groups: watermark effectiveness and whether text quality is affected.

### Detection Result

Table 1 shows paragraph-level detection results with no rewriting at all, and the result... is not actually better? The authors still spin it a bit: raw detectability is about the same as the others, so it is fine, and at least far better than SynthID.

![table 1 detection accuracy](table1-detection-accuracy.png)

Table 2 shows performance under attack, and this is where SWAN's advantage shows, because SWAN's watermark signal is the AMR structure of the sentence itself, so even after rewriting it does not change much. Other purely semantic approaches may drift under rewriting.

![](table2-paraphrase-robustness.png)

### Effect of AMR-Bank Size

Intuitively, a larger bank size covers more semantic patterns, but false positives could likewise be higher, because the LLM might just happen to generate an AMR structure similar to a secret bank template. Experiments here found that 800 and 50 are about the same, so you cannot say a bigger bank means higher AUC. The authors say performance should be the main consideration when setting the size. But honestly I find this rather puzzling. Why would that be?

![Table 3 effect of AMR-bank size](table3-amr-bank-size.png)

### Text Quality Evaluation

Then Claude scores the text produced by these methods, and the authors say SWAN is actually on par in quality with the other watermarking methods. The authors say that being on par while having much stronger paraphrase robustness is great.
![](table4-text-quality.png)

### Qualitative Examples

Qualitative analysis

SWAN's high example, although judged High, still has the repeated "Vote for ... Vote for ...". The Low one is even worse, what is with all the Johns. SemStamp's Low just looks a bit unnatural, while k-SemStamp has repeated information and odd punctuation.

![Table 5 actual examples generated by these methods, qualitative analysis](table5-qualitative-examples.png)
### Sampling Efficiency

Like other sentence-level algorithms, SWAN also relies on rejection sampling. Computing $250 \times 5 = 1250$ sentences, they conclude that 17.7 trials are needed on average. By comparison, SemStamp needs only 13.8 on average. Figure 3 shows the full distribution: 42% of sentences are accepted within 10 attempts, and 54% within 15. The peak at 46 to 50 corresponds to sentences that nearly exhaust the sampling budget (sentences that cannot possibly match the AMR template; in an Agent setting I think these would be tool-call outputs)

The authors say future work could do context-aware template selection instead of random selection, which might also reduce the number of inferences.
![Figure 3 rejection sampling trials](fig3-rejection-sampling-trials.png)

## Conclusion

- We introduced SWAN, a novel semantic watermarking framework that embeds robust, paraphrase-resistant signals into AMR representations. The authors propose SWAN, a new semantic watermarking framework that embeds signals into the AMR representation.
- Empirically, it outperforms token-level and embedding-based watermark baselines in paraphrase robustness, while maintaining strong text fluency and naturalness. Empirically, SWAN's paraphrase robustness is the best.

Some limitations
- The AMR Parser itself makes mistakes
- How to securely manage the Secret Bank?
- Only works for English news

## My Thoughts

For code or JSON output, this does not seem to be covered well, right? And it is even less suitable for Agent scenarios. The number of inferences is also a problem: the cost of one LLM forward pass is acceptable, but many is not.

In Related Work the authors mention that early linguistic steganography also hid information via syntax trees, which is very similar to SWAN's concept. I should be able to make good use of this. I feel there must be some relationship across files in an Agentic System that I could use to hide information.
