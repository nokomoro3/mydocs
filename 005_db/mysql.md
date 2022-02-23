# MySQL

## utf8は本当のutf8ではない。

- utf8mb4を使う必要がある。設定としては、utf8mb4_general_ciでOK。

- これをしていないと、「Incorrect string value: ...」というエラーが出る場合がある。

- 参考
  - https://mita2db.hateblo.jp/entry/2020/12/07/000000
  - https://qiita.com/houtarou/items/228086bbc7d1d10abdb9

## mysql + sqlalchemyでタイムスタンプを小数点表現する方法

- sqlalchemyの普通のDATETIMEではなく、dialects.mysql.DATETIMEクラスを使い、fspという引数を指定する。
```python
from sqlalchemy import Column, Integer
# from sqlalchemy import DATETIME # こちらは使えない
from sqlalchemy.dialects.mysql import DATETIME
from app.db.base_class import Base
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class SampleModel(Base):
    id         = Column(Integer         , nullable=False, primary_key=True, autoincrement=True)
    updated_at = Column(DATETIME(fsp=6) , nullable=False)
```

- 参考
  - https://stackoverflow.com/questions/29711102/sqlalchemy-mysql-millisecond-or-microsecond-precision

## 外部キー制約の種類について

- https://qiita.com/suin/items/21fe6c5a78c1505b19cb