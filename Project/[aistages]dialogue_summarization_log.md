# Dialogue Summarization Competition

## Log

### 2024. 03. 14(목)

* Papago 번역 크롤링 수행 중...
* Bart & T5 모델 비교 코딩 완료
  ```python     
    if "bart" in config["general"]["model_name"] :
        print('-'*10, 'Bart Model Load', '-'*10,)
        bart_config = BartConfig().from_pretrained(model_name)
        generate_model = BartForConditionalGeneration.from_pretrained(config['general']['model_name'],config=bart_config)
    elif "t5" in config["general"]["model_name"] :
        print('-'*10, 'T5 Model Load', '-'*10,)
        t5_config = T5Config.from_pretrained(model_name)
        generate_model = T5ForConditionalGeneration.from_pretrained(config['general']['model_name'],config=t5_config)
    ```
    * T5 base 모델과 Bart base 모델은 Config와 ConditionalGeneration이 다름 따라서 모델에 맞게 맞줘야함
    ```python
    wandb.init(dir=config['wandb']['dir'])
    ```
    * wandb에 init할 때 dir로 로컬에서 wandb 저장경로 변경 가능
    ```python
    'save_strategy': 'epoch',
    'save_total_limit': 5
    ```
    * 'save_strategy'는 checkpoint 저장 기준을 뜻하고, 'save_total_limit'는 최대 checkpoint 저장 갯수를 뜻함, 최대 갯수를 넘으면 생성된 지 가장 오래된 순서로 삭제
* Google Translate를 통해 Dataset 영어 번역 완료
  * Google Translate는 10 MB까지 파일 번역 가능
  * Data를 .xlsx로 바꾼 후 번역하고 .xlsx을 다시 .csv로 바꿈
  * 원인 모를 띄어쓰기가 삽입되므로 columns name를 수정하고 str.strip()을 실행

### 2024. 03. 18(화)

* Model Compare 코드에서 T5 base Model이 OOM(Out Of Memory) 문제가 발생하므로 batch_size를 수정해서 코드를 동작

```python
"per_device_train_batch_size": 15,
"per_device_eval_batch_size": 32
```
