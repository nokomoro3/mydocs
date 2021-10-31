### FastAPI

#### 全般

- https://qiita.com/qiuyin/items/ce6a3c31d13a5403ad67

#### BASIC認証を実装する

- APIのエンドポイントに、ユーザ認証の依存性を注入する。

```python
@router.get("/helloworld")
async def get_helloworld(is_auth: bool = Depends(authenticate_user)):
    return "HelloWorld"
```


- 参考
  - https://lonesec.com/2020/01/12/user-register-with-fastapi_auth