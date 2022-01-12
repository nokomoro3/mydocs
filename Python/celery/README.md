# celery

## logging

- root_loggerとtask_loggerが準備されており、それぞれデコレータを使った関数で設定を変えられる。
  - https://hawksnowlog.blogspot.com/2020/12/how-to-customize-celery-default-logger.html#%E3%82%BF%E3%82%B9%E3%82%AF%E5%81%B4%E3%81%A7%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B%E3%83%AD%E3%82%AC%E3%83%BC%E3%81%AF-gettasklogger-%E3%82%92%E4%BD%BF%E3%81%86

- celery用のformatterも使える。以下が参考になる。
  - https://www.distributedpython.com/2018/11/06/celery-task-logger-format/