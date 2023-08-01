# 設置 API 金鑰

首先，請將您的 OpenAI API 金鑰填入以下代碼：

```python
import openai
openai.api_key = "YOUR_API_KEY"
```
# 上傳訓練數據文件
接下來，請將訓練數據文件上傳到 OpenAI 的服務器進行 Fine-Tune。這裡使用了名為 data/finance_train.jsonl 的訓練數據文件。
```python
training_file = "data/finance_train.jsonl"
upload_response = openai.File.create(
    file=open(training_file, "rb"),
    purpose='fine-tune'
)
print(upload_response)
```

# 開始 Fine-Tune
使用上傳的數據文件進行 Fine-Tune。
```python
fine_tune_response = openai.FineTune.create(
    training_file=upload_response.id,
)
fine_tune_response
```

# 檢查 Fine-Tune 結果
檢查 Fine-Tune 是否成功。如果成功，我們可以獲取 Fine-Tuned 模型的 ID。
```python
if fine_tune_response.fine_tuned_model != None:
    fine_tuned_model = fine_tune_response.fine_tuned_model
```

# 生成 Completion
最後，使用 Fine-Tuned 模型生成文本 Completion。在這個例子中，我們提供了一個示例文本 tmp 作為提示，以生成相應的文本 Completion。
```python
itmp = "人工智能技術在金融領域的應用將帶來巨大的變革 新興市場的經濟增長潛力引起了投資者的關注 數字貨幣的興起正在改變傳統金融交易方式 把這三條評論做摘要"

answer = openai.Completion.create(
    model=fine_tuned_model,
    prompt=tmp,
    max_tokens=10,  # 更改 tokens 數量以生成更長的 completion
    temperature=0
)
print(answer['choices'][0]['text'])
```