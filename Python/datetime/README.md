# datetime

- 現在
```python
from datetime import datetime
now = datetime.now()
print(now)
# OUT: 2021-11-14 05:27:16.873830
```

- timezoneを変更
```python
now = now.astimezone(timezone('Asia/Tokyo'))
print(now)
# OUT: 2021-11-14 14:27:16.873830+09:00
```

- 現在(timezone付き)
```python
from pytz import timezone
now = datetime.now(timezone('Asia/Tokyo'))
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

- 参考
  - Python で日付のフォーマットを変換する方法
    - https://leben.mobi/blog/python_date_format/python/
  - 日付の表記に関するノート
    - https://www.kanzaki.com/docs/html/dtf.html#jp-eur
