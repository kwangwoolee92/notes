flask 프로젝트 생성 및 설치
==============

## 1. 프로젝트 폴더 생성
    mkdir flask_project
    cd flask_project

## 2. 가상 환경 생성 및 활성화
    python -m venv venv
    source venv/bin/activate  # Unix 또는 macOS
    # 또는
    venv\Scripts\activate  # Windows

## 3.  Flask 설치
    pip install Flask
        
## 4. app.py 파일 생성 및 다음 코드 작성
    @app.py

    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def hello():
        return 'Hello, Flask!'

    if __name__ == '__main__':
        app.run(debug=True)    

## 5. 환경 변수 설정
    export FLASK_APP=app.py
    export FLASK_ENV=development

## 6. Flask 서버 실행
    flask run

    모듈 이름을 지정해서 할시
    flask --app 모듈명 run

