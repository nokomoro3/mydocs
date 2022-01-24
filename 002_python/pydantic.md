# Pydantic

## SQLAlchemyのORMモデルとの連携

```python

class UserOrm(Base):
    __table_args__ = {
        'mysql_engine': 'InnoDB',
        'mysql_charset': 'utf8mb4',
        'mysql_collate': 'utf8mb4_general_ci'
    }
    
    # ...
 
class UserModel(BaseModel):

    # ...
    class Config:
        orm_mode=True


user: UserOrm = UserOrm()

# ORM to Pydantic Model
user_model: UserModel = UserModel.from_orm(user)

# Pydantic to dict
user_model.dict()

# Pydantic replace
user_model2: UserModel = UserModel(**user_model.dict())
```
