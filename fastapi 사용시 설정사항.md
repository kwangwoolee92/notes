FastApi 설정
==============
- ## 1. python 설정
   ```
   python -m venv backend(가상환경 이름)
   source backend/Scripts/activate
   ```     

- ## 2. 설치할 라이브러리들(backend)
  ```
  pip install fastapi 
  pip install uvicorn
  pip install sentry_sdk
  pip install sqlmodel
  pip install jwt
  pip install PyJWT
  pip install passlib
  pip install pydantic_settings
  pip install pydantic[email]
  pip install psycopg
  pip install psycopg_binary
  pip install emails
  pip install jinja2
  pip install python-multipart
  pip install openai
  pip install utcnow
  pip install datetime
  pip install transformers
  pip install torch
  pip install pipline
  pip install BaseModel
  pip install bcrypt  
  pip install itsdangerous
  pip install openai
  pip install captcha
  pip install redis
  pip install aioredis 
  pip install fastapi-users[redis]
  pip install aiohttp
  pip install langchain
  ``` 
- ## 3. 실행 명령어 (backend)
    ```
    uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
    
    독커 빌드(image 생성)
    docker build --tag abclab-dev-portal-backend .

    docker run --env-file ./.env -it -p 8000:80 abclab-dev-portal-backend
    (.env 파일을 참조하여 실행)  
    ```

- ## 4. 실행 명령어 (frontend)
  ```
  vite
  npm run dev -- -host 0.0.0.0 --port 80
  
  독커 컨테이너 생성 및 실행행
  docker run  -p 80:80 -e VITE_API_URL="http://192.168.0.150:8000"  abclab-dev-portal-frontend
  ```
  
 - ## 5. 오류사항
   - ### 5.1 docker 백엔드 문제
    ```  
    prestart.sh 안에 /r/n을 독커에서 인식못하여 파일을 CRLF 에서 LF 로 바꿔줘야 인식함
    ```
  