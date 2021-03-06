# SQLiScanner

[![GitHub issues](https://img.shields.io/github/issues/0xbug/SQLiScanner.svg)](https://github.com/0xbug/SQLiScanner/issues)
[![GitHub forks](https://img.shields.io/github/forks/0xbug/SQLiScanner.svg)](https://github.com/0xbug/SQLiScanner/network)
[![GitHub stars](https://img.shields.io/github/stars/0xbug/SQLiScanner.svg)](https://github.com/0xbug/SQLiScanner/stargazers)
[![Python 3.x](https://img.shields.io/badge/python-3.x-yellow.svg)](https://www.python.org/) 
[![GitHub license](https://img.shields.io/badge/license-GPLv3-blue.svg)](https://raw.githubusercontent.com/0xbug/SQLiScanner/master/LICENSE)

> Automatic SQL injection with Charles and sqlmapapi

## Introduction

从 优信二手车内部安全平台 分离出来的一个模块, 支持 **Har** 文件的扫描(搭配 Charles 使用: Tools=>Auto Save)

## Dependencies

- Django
- PostgreSQL
- Celery
- sqlmap
- redis

## Supported platforms

- Linux
- osx

## Screenshots
![](http://obfxuk8r6.bkt.clouddn.com/ScreenshotsUpload.png)

![](http://obfxuk8r6.bkt.clouddn.com/ScreenshotsResults.png)
## Installation
Preferably, you can download SQLiScanner by cloning the Git repository:
```
git clone https://github.com/0xbug/SQLiScanner.git --depth 1
```

You can download sqlmap by cloning the Git repository:
```
git clone https://github.com/sqlmapproject/sqlmap.git --depth 1
```

SQLiScanner works with Python version 3.x on Linux and osx.

Create virtualenv and install requirements
```
cd SQLiScanner/
virtualenv --python=/usr/local/bin/python3.5 venv
source venv/bin/activate
pip install -r requirements.txt
```
Syncdb
```
python manage.py makemigrations scanner
python manage.py migrate
```

Create superuser

```
python manage.py createsuperuser
```

## Setting

DATABASES Setting
```
SQLiScanner/settings.py:85
```

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': '',
        'USER': '',
        'PASSWORD': '',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

SendEmail Setting
```
SQLiScanner/settings.py:152
```
```
# Email

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_USE_TLS = False
EMAIL_HOST = ''
EMAIL_PORT = 25
EMAIL_HOST_USER = ''
EMAIL_HOST_PASSWORD = ''
DEFAULT_FROM_EMAIL = ''
```


```
scanner/tasks.py:13
```

```
class SqlScanTask(object):
    def __init__(self, sqli_obj):
        self.api_url = "http://127.0.0.1:8775"
        self.mail_from = ""
        self.mail_to = [""]

```

## Run

```
redis-server
python sqlmapapi.py -s -p 8775
python manage.py celery worker --loglevel=info
python manage.py runserver
```
