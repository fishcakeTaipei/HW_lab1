#112-2教育資料探勘專題
##HW1

> Lab 1 - Exploring the Power of LLM

Code：
```js
import requests
import json  

curl = "https://ntnu-ml.openai.azure.com/openai/deployments/ntnu-ml-gpt4-32k/chat/completions?api-version=2024-02-15-preview"

headers = {
    "Content-Type": "application/json",
    "api-key": "7c196b48c6c14f25adc5a8d7dcbc8d02"   # 使用老師的API Key
}

data = {
    "messages": [
        {"role": "system", "content": "根據 `` 中提供的 rating.csv 資料，用協同過濾的概念推薦餐廳給使用者  ，請以 json array 格式回答\n\`\ncustomerId,restaurantId,rating\nc1,r2,3\nc1,r3,1\nc1,r5,3\nc1,r6,2\nc2,r1,3\nc2,r3,1\nc2,r5,1\nc2,r6,1\nc3,r4,3\nc3,r5,3\nc3,r6,3\nc4,r1,1\nc4,r4,1\nc4,r5,3\nc5,r2,1\nc5,r3,2\nc5,r4,3\nc6,r2,3\nc6,r3,3\nc6,r5,3\nc7,r2,3\nc7,r3,3\nc7,r4,1\nc8,r1,2\nc8,r2,1\nc8,r5,1\nc8,r6,2\n\`，舉例來說回傳的結果中需要包含推薦的餐廳的欄位，像是「recommendations:r1、r2」。"},
        {"role": "user", "content": "c1"}
    ],
    "max_tokens": 800,
    "temperature": 0.5,
    "frequency_penalty": 0,
    "presence_penalty": 0,
    "top_p": 0.95,
    "stop": None
}

response = requests.post(curl, json=data, headers=headers)
response_data = response.json()

# 嘗試提取restaurantId的資料
restaurant_ids = []
for choice in response_data["choices"]:
    message_content = choice["message"]["content"]
    if message_content.strip():  # 檢查 message_content 是否為空
        json_content = json.loads(message_content)
        for recommendation in json_content:
            restaurant_ids.extend(recommendation.get('recommendations', []))

# 輸出提取到的restaurantId
print("提取到的 restaurantId:", restaurant_ids)

# 輸出完整的回應資料
print("完整的回應資料:", response_data)

```
Video：
<https://youtu.be/lOgaXgeZ47U>
