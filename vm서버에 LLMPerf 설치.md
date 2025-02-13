vm서버에 LLMPerf 설치
==============

## 1. vm 선행 설치
```
sudo apt install git
git config --global user.name KwangWooLee
git config --global user.eamil KwangWooLee@shiftone.kr
sudo apt install python3-pip
pip install --upgrade pip
```
        
***
## 2. 설치
```
git url : https://github.com/ray-project/llmperf

git clone https://github.com/ray-project/llmperf.git
cd llmperf
pip install -e .
```
