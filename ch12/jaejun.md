• 웹사이트에 있는 개인의 프로필이나 개인이 작성한 문서는 해당 소유자의 동의 없이는 권한이 없는 사용자가 볼 수 없어야한다.

```jsx
from flask import Flask, request, Response

app = Flask(__name__)

# 가정용 사용자 데이터베이스 (간단하게 하기 위해 하드코딩)
users = {
    "user1": "password1",
    "user2": "password2"
}

# 인증이 필요한 라우트
@app.route('/private')
def private():
    auth = request.authorization
    if not auth or not check_auth(auth.username, auth.password):
        return authenticate()
    return "Welcome to the private area!"

# 인증 확인 함수
def check_auth(username, password):
    return username in users and users[username] == password

# 인증 요청 함수
def authenticate():
    return Response(
        'Authentication required.', 401,
        {'WWW-Authenticate': 'Basic realm="Family"'}
    )

if __name__ == '__main__':
    app.run(debug=True)

```

```jsx
from flask import Flask, request, jsonify
import jwt
from datetime import datetime, timedelta, timezone

app = Flask(__name__)

# 가정용 사용자 데이터베이스 (간단하게 하기 위해 하드코딩)
users = {
    "user1": "password1",
    "user2": "password2"
}

# JWT 시크릿 키 (실제로는 보안에 민감한 정보이므로 환경 변수 등을 사용하여 숨겨야 함)
SECRET_KEY = 'your_secret_key'

# 로그인 엔드포인트
@app.route('/login', methods=['POST'])
def login():
    data = request.json
    username = data.get('username')
    password = data.get('password')

    if not username or not password or not check_auth(username, password):
        return jsonify({'message': 'Login failed!'}), 401

    token = jwt.encode({'username': username, 'exp': datetime.now(timezone.utc) + timedelta(minutes=30)}, SECRET_KEY, algorithm='HS256')
    return jsonify({'token': token}), 200

# 가정: JWT를 통한 보호된 엔드포인트
@app.route('/private')
def private():
    token = request.headers.get('Authorization')
    if not token:
        return jsonify({'message': 'Token is missing!'}), 401

    try:
        decoded_token = jwt.decode(token.split(' ')[1], SECRET_KEY, algorithms=['HS256'])
        return jsonify({'message': 'Welcome to the private area, {}!'.format(decoded_token['username'])}), 200
    except jwt.ExpiredSignatureError:
        return jsonify({'message': 'Token has expired!'}), 401
    except jwt.InvalidTokenError:
        return jsonify({'message': 'Invalid token!'}), 401

# 인증 확인 함수
def check_auth(username, password):
    return username in users and users[username] == password

if __name__ == '__main__':
    app.run(debug=True)

```

pip3 install pyjwt

pip3 install jwt

pip3 install flask

```jsx
curl --location --request POST 'http://127.0.0.1:5000/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username":"user1",
    "password":"password1"
}'
```

```jsx
curl --location --request GET 'http://127.0.0.1:5000/private' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InVzZXIxIiwiZXhwIjoxNzE2ODIzNzc1fQ.5JJ25wdqXZQcZATsHzil1Sqth_ecdl52YdL0GwuRmBQ'
```