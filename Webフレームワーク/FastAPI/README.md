# FastAPI

## 全般

- https://qiita.com/qiuyin/items/ce6a3c31d13a5403ad67

## BASIC認証を実装する

- APIのエンドポイントに、BASIC認証の依存性を注入する。
- パスワードのハッシュ用に、passlib[bcrypt]のPythonパッケージが必要

```python
from fastapi import FastAPI, Depends, HTTPException
from fastapi.security import HTTPBasic, HTTPBasicCredentials
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

user_db_mock = {
    "test@example.com": {
        "hashed_password": pwd_context.hash("password"),
    }
}

def get_user_by_email(email: str):
    return user_db_mock[email]

def authenticate_user(email: str, password: str):
    user = get_user_by_email(email)
    if not user:
        return False
    if not pwd_context.verify(password, user["hashed_password"]):
        return False
    return True

def authenticate_basic(
        credentials: HTTPBasicCredentials = Depends(HTTPBasic())):
    if authenticate_user(credentials.username, credentials.password):
        return True
    else:
        raise HTTPException(
                status_code=401,
                detail="Incorrect email or password",
                headers={"WWW-Authenticate": "Basic"})

app = FastAPI()

@app.get("/")
def read_root(is_auth: bool = Depends(authenticate_basic)):
    return {"Hello": "World"}
```

- 参考
  - https://lonesec.com/2020/01/12/user-register-with-fastapi_auth

## ファイルダウンロードAPIを作る

```python
from fastapi import FastAPI
from fastapi.responses import FileResponse

app = FastAPI()

@app.get("/")
async def main():
    file_path = "file_path"
    filename = "filename"
    return FileResponse(path=file_path, filename=filename)
```

- 参考
  - https://fastapi.tiangolo.com/advanced/custom-response/?h=fileresponse#fileresponse

## ロガーの設定

- https://gitter.im/tiangolo/fastapi?at=5ee330e3013105125a2e128d