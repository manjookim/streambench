# streambench
streambench 를 GPU에 적용해보는 실습 


------------------------------         
## 1. python llama 서버 설치   
GPU 포함 
```
CMAKE_ARGS="-DLLAMA_CUBLAS=on" \
  pip install --no-cache-dir --force-reinstall llama-cpp-python[server]
```

<br>


## 2. llama 서버 실행         
```
python -m llama_cpp.server \
  --model tinyllama-1.1b-chat-v1.0.Q4_K_M.gguf \
  --n_ctx 2048 \
  --n_batch 256  --n_gpu_layers 999 \
  --host 127.0.0.1 --port 8080
```
<br>

## 3. 로컬 llm 모델 다운로드         
hugging face 접속해서 원하는 모델 다운로드    
<br>
[https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF](https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF)

<br>

## 4. streambench github clone           
```
git clone https://github.com/stream-bench/stream-bench
```
<br>

## 5. streambench 환경설정           
- 가상환경 설치 및 접속
```
python3 -m venv venv        
source venv/bin/activate
```

- 필요한 패키지 다운로드 
```
pip install -r requirements.txt
```

- `configs/agent/zeroshot.yaml` 파일 수정
```
llm:
	series :"openai"
	model_name :"tinyllama-local"
```

- key, base 설정
```
export OAI_KEY="local"
export OAI_BASE="http://127.0.0.1:8080/v1"
```
<br>

## 6. streambench 실행         

```
# 가상환경 활성화 후(venv 추천) 레포 디렉토리에서:
cd ~/streambench/stream-bench
python -m stream_bench.pipelines.run_bench \
  --agent_cfg configs/agent/zeroshot.yml \
  --bench_cfg configs/bench/ddxplus.yml
```

------------------------
### References
[streambench github](https://github.com/stream-bench/stream-bench)
[streambench paper](https://arxiv.org/abs/2406.08747)
