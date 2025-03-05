flask wsgi 실행
==============

## 1. 윈도우에서 wsgi 실행
### 1.1 Waitress 설치
    pip install waitress

### 1.2 waitress 코드 생성
    ex) app.py
    from flask import Flask

    app = Flask(__name__)

    @app.route('/')
    def hello():
        return "Hello, World!"

    if __name__ == '__main__':
        from waitress import serve
        serve(app, host='0.0.0.0', port=8080)

### 1.3 실행  
    python app.py

### 단점
  - 소스가 변경 될시 리로딩되는 기능을 제공하지 않음

### 2.1 mod_wsgi 설치 
    pip install mod_wsgi

### 2.2 실행    
    mod_wsgi-express start-server app.py