docker 자주 쓰는 명령어
==

### 1. docker pull redis
 - 도커에 redis 설치 
 - redis 는 메모리적 DB임
 - docker run -p 6379:6379 --name docker_redis redis
 
  
### 1. 독커 안에 진입 명령어
    docker exec -it  hostname /bin/bash
    docker exec -it  582c9004e293 /bin/bash
  
### 2. 독커 빌드(image 생성) 명령어
    docker build --tag abclab-dev-portal-backend .
  
### 3. 독커 컨테이서 생성및 실행
    docker run  -p 8000:8000 abclab-dev-portal-backend
      
    docker run --env-file ./.env -it -p 8000:80 abclab-dev-portal-backend
    (.env 파일을 참조하여 실행)
  