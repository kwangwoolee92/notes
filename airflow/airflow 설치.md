airflow 설치
==============

## 1. 가상 환경 생성 및 활성화
    python3 -m venv airflow-env
    source airflow-env/bin/activate
## 2. pip 업그레이드
    pip install --upgrade pip

## 3.  Airflow 설치
    AIRFLOW_VERSION=2.10.4
    PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
    CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
    pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
    
## 4. Airflow 홈 디렉토리 설정
    export AIRFLOW_HOME=~/airflow

## 5. 데이터베이스 초기화
    airflow db init

## 6. 관리자 계정 생성
    airflow users create --username admin --firstname FIRST_NAME --lastname LAST_NAME --role Admin --email admin@example.com

## 7. Airflow 웹서버 및 스케줄러 시작
    airflow webserver -p 8080 &
    airflow scheduler &     

## 8. 디폴트인 sqllite 에서 postgresql 로 변경 하는법
    #postgresql 어뎁터 설치
    pip install psycopg2-binary

    #DB 경로 설정
    airflow.cfg의 sql_alchemy_conn 부분을 postgresql 연결자로 변경 
    ex)
    sql_alchemy_conn = postgresql+psycopg2://postgres:3qEr4nQdM3@192.168.0.34:31895/flasktest

    # DB 초기화
    airflow db init

    # 계정 설정
    airflow users create \
    --username admin \
    --firstname [이름] \
    --lastname [성] \
    --role Admin \
    --email [이메일] \
    --password [비밀번호]