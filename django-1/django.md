---
description: Django 설치와 mysql 연동
---

# Django 시작하기

django는 기본적으로 sqlite를 지원하지만 mysql, mariadb등 외부 db와 연동이 가능하다.

## 공식문서

{% embed url="https://docs.djangoproject.com/ko/2.2/intro/tutorial01/" %}

{% code title="Django 설치하기" %}
```text
python -m pip install Django
```
{% endcode %}

{% code title="프로젝트 시작하기 \(settings,  manage.py\)" %}
```text
django-admin startproject backend
```
{% endcode %}



![startproject backend &#xC2E4;&#xD589;](../.gitbook/assets/image%20%2833%29.png)

> 생성한 프로젝트로 들어가기 cd backend

{% code title="app 생성 \( model, admin\)" %}
```text
django-admin startapp api
```
{% endcode %}



![&#xC0DD;&#xC131;&#xB41C; api\(myapp\)](../.gitbook/assets/image%20%2814%29.png)

## Settings 수정하기.

> Settings.py 설정 추가하기  
> backend / backend / settings.py

{% code title="rest\_framework와 api를 추가한다." %}
```text
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # restframework
    'rest_framework',
    # apps
    'myapp',
]
```
{% endcode %}

```text
ALLOWED_HOSTS = ["localhost","127.0.0.1"]
```

## URL 추가하기.

{% code title="api의 urls를 가져다 쓰겠다고 선언하기" %}
```text
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('api.urls')),
]

```
{% endcode %}

{% code title="api에 api를 수행할 소스들을 담을 폴더 \(views\)를 생성하기 test.py" %}
```text
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response

@api_view(['GET','POST'])
def index(request):
    if request.method=='GET':
        request_data=["get"]
        return Response(data=request_data, status=status.HTTP_200_OK)
    elif request.method=='POST':
        request_data=["POST"]
        return Response(data=request_data, status=status.HTTP_200_OK)

```
{% endcode %}

> Get 방식과 Post 방식을 쓰겠다고 선언\(틀만 만들어놓음\)

![&#xACB0;&#xACFC;&#xBB3C;](../.gitbook/assets/image%20%289%29.png)

{% code title="backend / api / urls.py" %}
```text
from django.conf.urls import url
from .api_views import test
urlpatterns = [
    url('index/$', test.index, name='index'),
]

```
{% endcode %}

> api\_views 폴더의 test.py를 import  
> localhost:8080/index/라고 호출하면 test.py의 index함수를 호출하겠다고 선언.

## mysql 연동하기. 

{% code title="mysql 연동 클라이언트 설치하기" %}
```text
pip install mysqlclient
```
{% endcode %}

> database 연동하기.  
> backend / backend / settings.py

![&#xAE30;&#xC874;](../.gitbook/assets/image%20%2811%29.png)

![mysql&#xC758; junggobi&#xB77C;&#xB294; &#xD14C;&#xC774;&#xBE14;&#xACFC; &#xC5F0;&#xACB0;&#xD558;&#xACA0;&#xB2E4;](../.gitbook/assets/image%20%2835%29.png)

```text
DATABASES = {
    'default': {
        'ENGINE':'django.db.backends.mysql',
        'NAME': 'junggobi',
        'USER': 'newuser',
        'PASSWORD': 'ssafy',
        'HOST': '192.168.100.60',# Or an IP Address that your database is hosted on
        'PORT': '3306'
    }
}
```

## 설정한 것들 반영하기.

```text
python manage.py migrate
```

![&#xC96C;&#xB974;&#xB974;&#xB974;&#xB974;&#xB975;](../.gitbook/assets/image%20%2845%29.png)

![&#xC131;&#xACF5;&#xC801;&#xC73C;&#xB85C; mysql&#xACFC; &#xC5F0;&#xB3D9;&#xB418;&#xC5C8;&#xB2E4;!](../.gitbook/assets/image%20%2827%29.png)

