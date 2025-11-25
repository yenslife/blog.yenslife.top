
+++
date = '2025-11-05T16:09:25+08:00'
draft = false
title = 'Turn gpt-4o-mini into Your Maid: Fine-Tune gpt-4o-mini with JSONL and Use It in Open WebUI! (Includes Open WebUI Pipeline Tutorial)'
+++

This is a test article. If you’re interested in my Ironman competition articles, feel free to check out the [series](https://ithelp.ithome.com.tw/users/20168885/ironman/7699).

## Introduction

Three days left! Today, let’s explore OpenAI’s fine-tuning feature.  
The dataset I’m using is the one I created during a past project — it simulates the tone of a cute maid XD. I originally fine-tuned the model locally, but now OpenAI’s fine-tuning and inference prices have become incredibly affordable, almost like electricity or internet: basic infrastructure-level cheap.

![](https://i.imgur.com/xnwJuyy.png)

## Fine-Tuning

(Before you start, make sure you have topped up your OpenAI account!)

After [logging in](https://platform.openai.com/docs/overview), open the Dashboard.

![](https://i.imgur.com/2iNigYw.png)

Click **Fine-tuning** on the left. If you’ve never created a fine-tuning job before, this page will be empty.

![](https://i.imgur.com/tT7Efy0.png)

### Preparing the Dataset (JSONL)

You’ll need a dataset in **JSONL** — JSON Lines — which is similar to JSON, but each line contains a separate JSON object. It’s ideal for large-scale line-by-line reading.

Here is JSON:

```json
{
  "name": "Alice",
  "age": 30,
  "city": "New York"
}
````

Here is JSONL:

```json
{"name": "Alice", "age": 30, "city": "New York"}
{"name": "Bob", "age": 25, "city": "Los Angeles"}
{"name": "Charlie", "age": 35, "city": "Chicago"}
```

OpenAI’s fine-tuning format looks like this — essentially a chat history with system/user/assistant messages:

```json
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, {"role": "user", "content": "What's the capital of France?"}, {"role": "assistant", "content": "Paris, as if everyone doesn't know that already."}]}
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, {"role": "user", "content": "Who wrote 'Romeo and Juliet'?"}, {"role": "assistant", "content": "Oh, just some guy named William Shakespeare. Ever heard of him?"}]}
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, {"role": "user", "content": "How far is the Moon from Earth?"}, {"role": "assistant", "content": "Around 384,400 kilometers. Give or take a few, like that really matters."}]}
```

Here’s part of my maid-style dataset. Some lines were written by me; others were generated using GPT-4 XD.
It includes Western names and lots of emojis.

GitHub link:
[https://github.com/yenslife/it-ironman-2024/blob/main/Day28-openai-fine-tune/maid_finetune_output.jsonl](https://github.com/yenslife/it-ironman-2024/blob/main/Day28-openai-fine-tune/maid_finetune_output.jsonl)

```json
{"messages": [{"role": "system", "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"}, {"role": "user", "content": "嗨，莎拉，請向我介紹一下你自己吧。"}, {"role": "assistant", "content": "嗨，尊敬的主人！我是女僕莎拉，您的忠誠助手！🌟 我熱愛花藝和裝飾，喜歡為您打造一個舒適溫馨的家。🌺🏡 我還喜歡和您分享笑聲和歡樂，因為您的快樂就是我的快樂！😄❤️ 如有需要，請隨時告訴我，我將全心為您服務！"}]}
{"messages": [{"role": "system", "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"}, {"role": "user", "content": "女僕莉莉，你最喜歡做什麼甜點？"}, {"role": "assistant", "content": "主人您好！我最喜歡製作草莓蛋糕，它既美味又充滿甜蜜的香氣！🍓🍰 如果您喜歡，我隨時可以為您親手製作一個！❤️"}]}
```

### Start Fine-Tuning

Go back to the fine-tuning page and click **Create**.
Select the model you want to fine-tune. (You can also continue fine-tuning an already fine-tuned model.)

![](https://i.imgur.com/70eksmW.png)

Upload your file.

![](https://i.imgur.com/77YUwoo.png)

Leave the other fields at their defaults and click **Create**.

If anything fails, your file is probably not in valid JSONL format.

![](https://i.imgur.com/ejhdlxK.png)

Once fine-tuning finishes, you'll see **Succeeded** and the training metrics on the right.

![](https://i.imgur.com/AtlSiIs.gif)

### Testing

You can test directly via Playground,
or write a Python script.
Remember: **use the same system prompt you trained with** for best results.

![](https://i.imgur.com/c8fIogi.png)

Python test script:

```python
import openai
from dotenv import load_dotenv
import os

load_dotenv()

input_string = "你好ㄚ可愛的模型"
model_name = "ft:gpt-4o-mini-2024-07-18:personal::AGfhVX3D"

openai.api_key = os.getenv("OPENAI_API_KEY")

client = openai.OpenAI()
completion = client.chat.completions.create(
    model=model_name,
    messages=[
        {"role": "system", "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"},
        {"role": "user", "content": input_string}
    ]
)

print(completion.choices[0].message.content)
```

![](https://i.imgur.com/LXtSFm5.png)

## Using It in Open WebUI

To install Open WebUI, see **Day 10**:
[https://ithelp.ithome.com.tw/articles/10357750](https://ithelp.ithome.com.tw/articles/10357750)

If you’re using the same API key as the one used for fine-tuning, your fine-tuned model should appear in the model selection menu.

![](https://i.imgur.com/a5n0qF8.png)

But testing it may feel… cold.
Not the warm, affectionate maid we trained 😢

![](https://i.imgur.com/7VV98PR.png)

This is because the **system prompt isn’t set**.

Set it to:

> 你是一個可愛的女僕機器人，用繁體中文回答主人的問題。

![](Pasted image 20241010160319.png)

Try again — the cute, slightly clingy maid atmosphere returns 🥰

![](https://i.imgur.com/6o9ZFZr.png)

## Open WebUI Pipeline

Official documentation:
[https://docs.openwebui.com/pipelines/](https://docs.openwebui.com/pipelines/)

Using the above system prompt applies it to *every* model.
To avoid that, we can use **Pipeline**, which lets you route messages through custom logic.

A pipeline can connect *anything* that takes a string input and returns a string output — even a Dify workflow.

### Installation

You can start the Pipeline backend using Docker:

```bash
docker run -d -p 9099:9099 --add-host=host.docker.internal:host-gateway -v pipelines:/app/pipelines --name pipelines --restart always ghcr.io/open-webui/pipelines:main
```

Then in Open WebUI:

**Admin Settings → Connections → Add a new connection**

* URL: `http://host.docker.internal:9099`
* API Key: `0p3n-w3bu!`

![](https://i.imgur.com/B6il8iB.png)

Now go to **Pipelines**, and you can upload scripts.

![](https://i.imgur.com/3UA19nm.png)

### Writing the Pipeline Script

Here’s a modified version of the OpenAI example, with:

✔ A customized system prompt
✔ Removal of Open WebUI’s default system prompt
✔ Support for your fine-tuned model

```python
from typing import List, Union, Generator, Iterator
from schemas import OpenAIChatMessage
from pydantic import BaseModel
import os
import requests


class Pipeline:
    class Valves(BaseModel):
        OPENAI_API_KEY: str = ""
        pass

    def __init__(self):
        self.name = "OpenAI Pipeline Maid Mode"
        self.valves = self.Valves(
            **{
                "OPENAI_API_KEY": os.getenv(
                    "OPENAI_API_KEY", "YOUR_API_KEY_HERE"
                )
            }
        )
        pass

    async def on_startup(self):
        print(f"on_startup:{__name__}")
        pass

    async def on_shutdown(self):
        print(f"on_shutdown:{__name__}")
        pass


    def pipe(
        self, user_message: str, model_id: str, messages: List[dict], body: dict
    ) -> Union[str, Generator, Iterator]:

        OPENAI_API_KEY = self.valves.OPENAI_API_KEY
        MODEL = "YOUR_FINE_TUNED_MODEL_NAME"

        headers = {
            "Authorization": f"Bearer {OPENAI_API_KEY}",
            "Content-Type": "application/json",
        }

        system_prompt = {
            "role": "system",
            "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"
        }

        # Remove WebUI's default system prompt
        messages.pop(0)

        # Insert our system prompt
        messages.insert(0, system_prompt)

        payload = {**body, "model": MODEL, "messages": messages}

        for field in ["user", "chat_id", "title"]:
            payload.pop(field, None)

        try:
            r = requests.post(
                url="https://api.openai.com/v1/chat/completions",
                json=payload,
                headers=headers,
                stream=True,
            )

            r.raise_for_status()

            if body["stream"]:
                return r.iter_lines()
            else:
                return r.json()
        except Exception as e:
            return f"Error: {e}"
```

Upload your script (don’t forget to click **Upload**!)

![](https://i.imgur.com/5Fzkbx3.png)

![](https://i.imgur.com/rGEpgvj.png)

### Testing

![](https://i.imgur.com/NCm5JsK.png)

Today’s code has been synced to GitHub:
[https://github.com/yenslife/it-ironman-2024/tree/main/Day28-openai-fine-tune](https://github.com/yenslife/it-ironman-2024/tree/main/Day28-openai-fine-tune)

## Conclusion

Originally I wanted to write about integrating Dify workflows with Open WebUI. But after recalling OpenAI’s announcement that fine-tuning vision models was free before 10/31, I went to check the pricing again and—wow. It has become unbelievably cheap.

This makes me even more confident that **API-based LLM apps** will become the mainstream.
LLM inference is becoming as ubiquitous as electricity and Wi-Fi.

I’ve been writing a lot about building things from scratch…
Tomorrow, I’ll share some of my favorite AI tools instead.
We’re almost at the finish line!
