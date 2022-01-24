# Pythonにおける例外処理

## 基本構文

- 以下のように基底クラスExceptionを用いれば、SystemExit, KeyboardInterrupt以外のすべての例外をキャッチできる。
```python
try:
    # 例外が発生しうる処理
except Exception as e:
    # 例外発生時の処理
```

- 例外時のトレースバックを取得したい場合は、以下のようにする。
```python
import traceback
try:
    # 例外が発生しうる処理
except Exception as e:
    # 例外発生時の処理
    error_message = traceback.format_exc()
```

- elseで正常終了時の処理、finallyで終了時の処理を記述できる。
- 両方記載されて終了時の場合は、else側が先に実行される。
```python
try:
    # 例外が発生しうる処理
except Exception as e:
    # 例外発生時の処理
else:
    # 正常終了時の処理
finally:
    # 終了時の処理
```

- 例外別に処理を分けたい場合は、exceptを複数記述する。
```python
try:
    # 例外が発生しうる処理
except ZeroDivisionError as e:
    # ゼロ割の例外発生時の処理
except Exception as e:
    # その他の例外発生時の処理
else:
    # 正常終了時の処理
finally:
    # 終了時の処理
```

## カスタム例外

- 自作クラスを作成した場合、カスタム例外を定義したくなる。
  - よくあるケースとしては、Python外のバイナリを実行して、プロセスのエラーで例外を出したい場合など。

- Exceptionクラスを継承した、カスタム例外の空クラスを作成し、raiseすればよい。
```python

import subprocess
from subprocess import CompletedProcess

class MyCustomException(Exception):
    pass

class MyClass():
    def __init__():
        pass
    def myfunc(self):
        cp: CompletedProcess = subprocess.run(
            ['msgconvert', input_path_str, '--outfile', f'{input_path.with_suffix(".eml")}'],
            stdout=subprocess.PIPE, stderr=subprocess.PIPE,
        )
        if cp.returncode != 0:
            raise MyCustomException(f"msgconvert faild : {cp.stderr.decode()}")
```
