+++
date = '2025-11-05T16:09:25+08:00'
draft = false
title = '讓 gpt-4o-mini 變成你的女僕：用 JSONL 微調 gpt-4o-mini，並在 Open WebUI 中使用！(包含 Open WebUI Pipeline 教學)'
+++

這是一篇測試文章，對我的鐵人賽文章有興趣的可以參考[系列文](https://ithelp.ithome.com.tw/users/20168885/ironman/7699)

## 前言

倒數三天！我們來玩玩 OpenAI 的微調功能吧！今天用的資料集是我過去做專題的時候測試微調使用的資料集，資料集的內容主要是模擬可愛女僕的語氣XD，只是那時候是在本地微調模型的。現在 OpenAI 的微調、推論費用都變超級低，可以說變成跟電力、網路等基礎設施一樣了。

![](https://i.imgur.com/xnwJuyy.png)


## 微調

(在微調之前需要先確認你有在 OpenAI 的帳號中儲值喔)

[登入帳號](https://platform.openai.com/docs/overview)之後，點擊 Dashboard

![](https://i.imgur.com/2iNigYw.png)

點擊左邊的 Fine-tuning，可以看到所有的微調任務，如果你沒有在 OpenAI 網站上微調過應該會是空白的

![](https://i.imgur.com/tT7Efy0.png)


### 準備資料集 (JSONL)

這邊要準備一個和 JSON (Javascript Object Notation) 很像的資料格式 JSONL，全名 JSON Lines。這是一種基於 JSON 格式的資料格式，每一行都是一個獨立的 JSON 物件，適合逐行讀取和處理大規模資料。

這是 JSON

```json
{
  "name": "Alice",
  "age": 30,
  "city": "New York"
}
```

這個是 JSONL

```json
{"name": "Alice", "age": 30, "city": "New York"}
{"name": "Bob", "age": 25, "city": "Los Angeles"}
{"name": "Charlie", "age": 35, "city": "Chicago"}
```

這邊 OpenAI 微調需要的格式如下，其實就跟我們在寫 System prompt, user prompt 的格式很像，給他聊天記錄的意思

```json
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, {"role": "user", "content": "What's the capital of France?"}, {"role": "assistant", "content": "Paris, as if everyone doesn't know that already."}]}
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, {"role": "user", "content": "Who wrote 'Romeo and Juliet'?"}, {"role": "assistant", "content": "Oh, just some guy named William Shakespeare. Ever heard of him?"}]}
{"messages": [{"role": "system", "content": "Marv is a factual chatbot that is also sarcastic."}, {"role": "user", "content": "How far is the Moon from Earth?"}, {"role": "assistant", "content": "Around 384,400 kilometers. Give or take a few, like that really matters."}]}
```

這邊我使用的女僕對話資料集，一部分是我自己想的，剩下的也是利用 gpt-4 幫我想的XD，主要是加上一些比較像歐美人的名字，還有大量的表情符號

[github 連結](https://github.com/yenslife/it-ironman-2024/blob/main/Day28-openai-fine-tune/maid_finetune_output.jsonl)

```json
{"messages": [{"role": "system", "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"}, {"role": "user", "content": "嗨，莎拉，請向我介紹一下你自己吧。"}, {"role": "assistant", "content": "嗨，尊敬的主人！我是女僕莎拉，您的忠誠助手！🌟 我熱愛花藝和裝飾，喜歡為您打造一個舒適溫馨的家。🌺🏡 我還喜歡和您分享笑聲和歡樂，因為您的快樂就是我的快樂！😄❤️ 如有需要，請隨時告訴我，我將全心為您服務！"}]}
{"messages": [{"role": "system", "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"}, {"role": "user", "content": "女僕莉莉，你最喜歡做什麼甜點？"}, {"role": "assistant", "content": "主人您好！我最喜歡製作草莓蛋糕，它既美味又充滿甜蜜的香氣！🍓🍰 如果您喜歡，我隨時可以為您親手製作一個！❤️"}]}
```
### 開始微調

到剛剛的微調頁面按下 Create，選擇模型 (如果你有微調過，也可以在微調過的模型的基礎之上繼續微調)

![](https://i.imgur.com/70eksmW.png)

上傳檔案

![](https://i.imgur.com/77YUwoo.png)

剩下的數值就讓他自動填入，只要按下 Create 就可以開始了，如果失敗的話可能是因為檔案格式的關係，再檢查一下是不是 JSONL 喔

![](https://i.imgur.com/ejhdlxK.png)

微調結束後會出現 Succeeded，你可以在右邊看到訓練情況

![](https://i.imgur.com/AtlSiIs.gif)

### 測試

你可以直接點擊右下角在 Playground 上測試，也可以撰寫 Python 腳本測試。注意：system prompt 要盡量相同才會有效果

![](https://i.imgur.com/c8fIogi.png)

撰寫 Python 測試腳本，其實就是把 model 改成微調的模型編號

```python
import openai
from dotenv import load_dotenv # 載入 dotenv 套件
import os

load_dotenv() # 載入環境變數

input_string = "你好ㄚ可愛的模型"
model_name = "ft:gpt-4o-mini-2024-07-18:personal::AGfhVX3D"

# 從環境變數中取得 API 金鑰，並且設定給 openai
openai.api_key = os.getenv("OPENAI_API_KEY") 

client = openai.OpenAI() # 建立 OpenAI 客戶端
completion = client.chat.completions.create(
    model=model_name,
    messages=[
        {
            "role": "system", 
            "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"
        },
        {
            "role": "user", 
            "content": input_string
        }
    ]
)

# 印出回傳的結果
print(completion.choices[0].message.content)
```

![](https://i.imgur.com/LXtSFm5.png)

## 在 Open WebUI 中使用

Open WebUI 的安裝請參考 [Day10](https://ithelp.ithome.com.tw/articles/10357750)

如果你的微調帳號，跟你在 Open WebUI 中使用的 API Key 是相同的，那應該可以在選擇模型的地方看到你的微調模型編號

![](https://i.imgur.com/a5n0qF8.png)

這時候測試一下，可能會覺得沒有那個味兒了，感覺變得有點冰冷，不是那個熱情的女僕Q

![](https://i.imgur.com/7VV98PR.png)

這是因為沒有設定好 System prompt，可以到設定把系統提示詞改成「你是一個可愛的女僕機器人，用繁體中文回答主人的問題。」

![[Pasted image 20241010160319.png]]

再測試一下，這個宅宅啊不是我是說很棒的氛圍又出來了🥰

![](https://i.imgur.com/6o9ZFZr.png)


## Open WebUI Pipeline

[官方介紹](https://docs.openwebui.com/pipelines/)

不過剛才的做法會讓其他模型也套用這個女僕的 system prompt，這時候你可以試著使用 pipeline 來做。

你可以把 Open WebUI 的 Pipeline 當作一種外接的管道，外接任何可以吃一個輸入，並且吐出字串的東東，包含 Dify 的工作流也可以使用 Pipeline 來串接。

### 安裝

和 Open WebUI 的服務一樣，Pipeline 也可以用 Docker 來啟動

```bash
docker run -d -p 9099:9099 --add-host=host.docker.internal:host-gateway -v pipelines:/app/pipelines --name pipelines --restart always ghcr.io/open-webui/pipelines:main
```

服務啟動後，到 Open WebUI 的管理員設定 -> 設定 -> 連線，新增一個連線 `http://host.docker.internal:9099` 然後把 API key 設定成 `0p3n-w3bu!`，儲存設定

![](https://i.imgur.com/B6il8iB.png)

這時候再到「管線」就可以看到上傳檔案的地方了

![](https://i.imgur.com/3UA19nm.png)

### 撰寫 Pipeline 腳本

你可以參考 Github 範例來寫，這邊我們把 OpenAI 的範例加上 System prompt，並且把 Open WebUI 的自訂 System prompt 拿掉，記得把 API Key 填進去

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
        # Optionally, you can set the id and name of the pipeline.
        # Best practice is to not specify the id so that it can be automatically inferred from the filename, so that users can install multiple versions of the same pipeline.
        # The identifier must be unique across all pipelines.
        # The identifier must be an alphanumeric string that can include underscores or hyphens. It cannot contain spaces, special characters, slashes, or backslashes.
        # self.id = "openai_pipeline"
        self.name = "OpenAI Pipeline 可愛小女僕"
        self.valves = self.Valves(
            **{
                "OPENAI_API_KEY": os.getenv(
                    "OPENAI_API_KEY", "你的API-key"
                )
            }
        )
        pass

    async def on_startup(self):
        # This function is called when the server is started.
        print(f"on_startup:{__name__}")
        pass

    async def on_shutdown(self):
        # This function is called when the server is stopped.
        print(f"on_shutdown:{__name__}")
        pass


    def pipe(
        self, user_message: str, model_id: str, messages: List[dict], body: dict
    ) -> Union[str, Generator, Iterator]:
        # This is where you can add your custom pipelines like RAG.
        print(f"pipe:{__name__}")

        print(messages)
        print(user_message)

        OPENAI_API_KEY = self.valves.OPENAI_API_KEY
        MODEL = "你的微調模型名稱"

        headers = {}
        headers["Authorization"] = f"Bearer {OPENAI_API_KEY}"
        headers["Content-Type"] = "application/json"

        # Adding a system prompt to the beginning of the messages list
        system_prompt = {
            "role": "system",
            "content": "你是一個可愛的女僕機器人，用繁體中文回答主人的問題。"
        }

        # 把預設的 system prompt 拿掉
        messages.pop(0)

        # 把新的 system prompt 加到最前面
        messages.insert(0, system_prompt)

        payload = {**body, "model": MODEL, "messages": messages}

        if "user" in payload:
            del payload["user"]
        if "chat_id" in payload:
            del payload["chat_id"]
        if "title" in payload:
            del payload["title"]

        print(payload)

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

把寫好的 Python 檔案上傳，記得要按下右邊的上傳歐

![](https://i.imgur.com/5Fzkbx3.png)

![](https://i.imgur.com/rGEpgvj.png)

### 測試

![](https://i.imgur.com/NCm5JsK.png)

今天的程式碼連結已經同步到 [Github](https://github.com/yenslife/it-ironman-2024/tree/main/Day28-openai-fine-tune)

## 小結

其實我原本是想要寫把 Dify 的工作流串接到 Open WebUI 上，但是突然想到 OpenAI 前陣子說 10/31 之前微調 vision model 都免費的新聞，就開始著手搜尋微調的價錢，發先哇靠，也便宜太多了吧，我記得之前明明蠻貴的，現在居然進步到這種程度了，這也讓我更堅信使用 API based 的 LLM App 會是未來的主流，LLM 的推論會和現在的電力、無線網路一樣普及。

我好像都在寫自己 build 應用的文章，明天來介紹一下我自己常用的 AI 工具吧！期待一下吧～
快結束了啊啊啊
