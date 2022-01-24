# datetime

- modules
```python
from datetime import datetime
from dateutil import tz
```

- 現在
```python
now = datetime.now()
print(now)
# OUT: 2021-11-14 05:27:16.873830
```

- timezoneを変更
```python
now = now.astimezone(tz.gettz('Asia/Tokyo'))
print(now)
# OUT: 2021-11-14 14:27:16.873830+09:00
```

- 現在(timezone付き)
```python
now = datetime.now(tz.gettz('Asia/Tokyo'))
print(now)
# OUT: 2021-11-14 14:28:16.824288+09:00
```

- 文字列に変換
```python
datetime.strftime(now, "%Y-%m-%d_%H%M%S")
# OUT: '2021-11-14_052716'
```

- 文字列から変換1
```python
time_str = '2021/11/14 14:31:23'
dt = datetime.strptime(time_str, '%Y/%m/%d %H:%M:%S')
print(dt)
# OUT: 2021-11-14 14:31:23
```

- 文字列から変換2(米国表記)
```python
time_str2 = 'Nov 14 2021 2:35PM'
dt = datetime.strptime(time_str2,'%b %d %Y %I:%M%p')
print(dt)
# OUT: '2021-11-14 14:35:00'
```

- 文字列から変換(RFC2822形式)
```python
date_str3= 'Sun, 14 Nov 2021 14:45:00 +0900'
dt = datetime.strptime(date_str3,'%a, %d %b %Y %H:%M:%S %z')
print(dt)
# OUT: 2021-11-14 14:45:00+09:00
```

- timezoneの変更
```python
print(dt.astimezone(tz.gettz("UTC")))
# OUT: 2021-11-14 05:45:00+00:00
```

- timezoneの強制変更
```python
from datetime import datetime, timezone
print(dt.replace(tzinfo=tz.gettz("UTC")))
# OUT: 2021-11-14 14:45:00+00:00
```

- timezoneの削除
```python
print(dt.replace(tzinfo=None))
# OUT: 2021-11-14 14:45:00
```

- 時間差分
```python
from dateutil.relativedelta import relativedelta
from datetime import datetime

now = datetime.now()
one_month_ago = now - relativedelta(months=1)
print(now)
# OUT: 2021-12-08 00:04:41.047085
print(one_month_ago)
#$ OUT: 2021-11-08 00:04:41.047085

```

- ISO8601への変換
  - https://note.nkmk.me/python-datetime-isoformat-fromisoformat/

- 参考
  - Python で日付のフォーマットを変換する方法
    - https://leben.mobi/blog/python_date_format/python/
  - 日付の表記に関するノート
    - https://www.kanzaki.com/docs/html/dtf.html#jp-eur
  - Pythonで翌日や翌月みたいな日付の計算をする
    - https://qiita.com/dkugi/items/8c32cc481b365c277ec2
