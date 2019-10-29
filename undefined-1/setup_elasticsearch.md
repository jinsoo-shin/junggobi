# 엘라스틱서치 설치하기

## 주의사항.

* 맨 처음 작성때엔 6.X 버전에 시작했으나 중간중간 이슈가. \(DocType, JavaPath, meta가 index로 바뀐부분\) 발생하여  7.X버전으로 업그레이드 했습니다.

DocType가 import가 안되는 에러  
-&gt; 엘라스틱 서치 업데이트 후 Document로 변경합니다.

[Root mapping definition has unsupported parameters](https://github.com/sabricot/django-elasticsearch-dsl/issues/207) 에러 발생 해결법.  
엘라스틱 서치 맨처음 설치시에 6.6.2-&gt; 7.1.0으로 설치합니다.

\Common 문제.  
밑에 적어놓은 것처럼 해결해도 되지만 엘라스틱서치를 7.X로 업데이트하니 해결됨.

you cannot perform api calls on the default index  
-&gt; class Meta: index 에서 class Index: name 으로 변경되었다 이렇게 수정하면 해결됨. 

## 1. 엘라스틱 서치 설치하기.

{% embed url="https://www.elastic.co/kr/downloads/past-releases/elasticsearch-6-6-2" caption="window는 window로 설치하기" %}

{% embed url="https://www.elastic.co/kr/downloads/past-releases/elasticsearch-7-1-0" %}



![zip &#xD30C;&#xC77C;&#xC744; &#xD480;&#xC5B4;&#xC900;&#xB2E4;](../.gitbook/assets/image%20%2824%29.png)



> bin폴더의 elasticsearch.bat을 실행시킨다

![](../.gitbook/assets/image%20%2833%29.png)

> !?!? 내가 예상했던 그림은 started가 뜨는 것인데 \Common은 예상되지 않았습니다. 에러메시지가 떴다.

![&#xB0B4;&#xAC00; &#xC608;&#xC0C1;&#xD588;&#xB358; &#xADF8;&#xB9BC;....](../.gitbook/assets/image%20%2840%29.png)

> 찾아보니 JAVA path를 인식못해서 그렇다고한다.....
>
> 고로
>
> elasticsearch.bat에 밑의 코드를 추가해준다.

```text
SET "JAVA_HOME=C:\Program Files\Java\jdk1.8.0_191"
```

> 뭐가 많이 뜨긴했지만 그래도 맨마지막에 started가 떴다!!

![](../.gitbook/assets/image%20%2842%29.png)

> 제대로 동작하는지 postman으로 테스트해보았다.

![Yes!!](../.gitbook/assets/image%20%2815%29.png)





## 2. nori 설치하기.

![](../.gitbook/assets/image%20%2838%29.png)

> 저 파일을 실행시켜서 노리를 설치해보겠다.

![](../.gitbook/assets/image%20%289%29.png)

> 또다시.... java위치를 못 읽는 이슈가 발생했다.  
> 한번 더 path를 추가한다

```text
SET "JAVA_HOME=C:\Program Files\Java\jdk1.8.0_191"
```

![&#xC124;&#xCE58;&#xAC00; &#xB418;&#xC5C8;&#xB2E4;!!!!](../.gitbook/assets/image%20%2814%29.png)



## 3. 장고와 연동하기.

> Haystack은 Django에 대한 모듈 식 검색을 제공 하는 **훌륭한 오픈 소스 도구** 입니다. 불행히도 최신 버전의 Elasticsearch를 완벽하게 지원하지는 않습니다. 이를 극복하기 위해 Elasticsearch 원래 개발 팀이 만든 **elasticsearch-dsl** 을 사용 하여 새로운 릴리스를보다 잘 지원합니다.  
>   
> 출처 : [https://www.merixstudio.com/blog/elasticsearch-django-rest-framework/](https://www.merixstudio.com/blog/elasticsearch-django-rest-framework/)

>



![&#xC124;&#xCE58;&#xB41C; &#xBC84;&#xC804;](../.gitbook/assets/image%20%2813%29.png)

![&#xBC11;&#xC758; &#xBC84;&#xC804;&#xC740; &#xC2E4;&#xD328;&#xD588;&#xB2E4; 7.X&#xB97C; &#xC124;&#xCE58;&#xD558;&#xC2DC;&#xC624;.](../.gitbook/assets/image%20%2839%29.png)

{% code-tabs %}
{% code-tabs-item title="설치" %}
```text
pip install elasticsearch-dsl
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="settings.py에 추가한다." %}
```text
ELASTICSEARCH_DSL = {
    'default': {
        'hosts': 'elasticsearch:9200'
    },
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> app폴더위치에 search.py 파일을 추가한다.

{% code-tabs %}
{% code-tabs-item title="엘라스틱 설정에 대한 글로벌 연결 완료 " %}
```text
from elasticsearch_dsl.connections import connections

connections.create_connection()
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="인덱스를 생성할 대상을 정한다 " %}
```text
from elasticsearch_dsl.connections import connections
from elasticsearch_dsl import DocType, Text, Date, Integer

connections.create_connection()

class NavercafeIndex(DocType):
    id = Integer()
    category = Text()
    manufacturer = Text()
    model_name = Text()
    generation = Text()
    display = Text()
    cellular = Text()
    storage = Text()
    price = Integer()
    region = Text()
    date = Date()
    link = Text()
    img_src = Text()
    is_sell = Integer()
    title = Text()
    contents = Text()
class Meta:
    index = 'navercafe-index'

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="search.py에 추가한다." %}
```text
from elasticsearch.helpers import bulk
from elasticsearch import Elasticsearch
from api.models import Navercafe
def bulk_indexing():
    Navercafe.init()
    es = Elasticsearch()
    bulk(client=es, actions=(b.indexing() for b in Navercafe.objects.all().iterator()))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> 모델에서 무언가를 변경할 때마다 대량 인덱싱 `init()`만하고 싶기 때문에 모델을 ElasticSearch에 매핑하는 모델입니다. 그런 다음를 사용하여 ElasticSearch에 대한 연결을 생성 `bulk`할 인스턴스를 전달합니다 `Elasticsearch()`. 그런 다음 생성기를 전달 하여 일반 데이터베이스에있는 `actions=`모든 `BlogPost`오브젝트를 반복하고 `.indexing()`각 오브젝트 에서 메소드를 호출하십시오 . 왜 발전기인가? 생성기를 반복 할 객체가 많으면 먼저 메모리에로드 할 필요가 없기 때문입니다.
>
> 위의 코드에는 한 가지 문제가 있습니다. `.indexing()`아직 모델에 대한 방법 이 없습니다
>
> 라고함.....[https://www.freecodecamp.org/news/elasticsearch-with-django-the-easy-way-909375bc16cb/](https://www.freecodecamp.org/news/elasticsearch-with-django-the-easy-way-909375bc16cb/) 여기에서 ㅇㅇㅇ.

{% code-tabs %}
{% code-tabs-item title="최종 search.py" %}
```text
from elasticsearch_dsl.connections import connections
from elasticsearch_dsl import Document, Keyword, Text, Integer, Date,Search

from elasticsearch.helpers import bulk
from elasticsearch import Elasticsearch
from . import models
connections.create_connection(hosts=['localhost'])

class NavercafeIndex(Document):
    id = Integer()
    category = Text()
    manufacturer = Text()
    model_name = Text()
    generation = Text()
    display = Text()
    cellular = Text()
    storage = Text()
    price = Integer()
    region = Text()
    date = Date()
    link = Text()
    img_src = Text()
    is_sell = Integer()
    title = Text()
    contents = Text()
    
    class Index:
        name = 'navercafe-index'

def bulk_indexing():
    NavercafeIndex.init()
    es = Elasticsearch()
    bulk(client=es, actions=(b.indexing() for b in models.Navercafe.objects.all().iterator()))

def search(display):
    s = Search().filter('match', display=display)
    response = s.execute()
    return response
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> elastic 7.x 버전부터 class Meta: index가 class Index: name으로 변경되었다.





> 인덱싱 방법을 models.py에 추가해준다.

```text
#Add indexing method to BlogPost
def indexing(self):
    obj = NavercafeIndex(
    meta={'id': self.id},
    category=self.category,
    manufacturer=self.manufacturer,
    model_name=self.model_name,
    generation=self.generation,
    display=self.display,
    cellular=self.cellular,
    storage=self.storage,
    price=self.price,
    region=self.region,
    link=self.link,
    img_src=self.img_src,
    is_sell=self.is_sell,
    title=self.title,
    contents=self.contents,
    )
    obj.save()
    return obj.to_dict(include_meta=True)
```



{% code-tabs %}
{% code-tabs-item title="인덱싱!!!!!!" %}
```text
python manage.py shell
from api.search import *
bulk_indexing()
```
{% endcode-tabs-item %}
{% endcode-tabs %}



> 제대로 들어갔는지 확인을 위해 작성한 search 함수를 실행한다

![&#xD06C;&#xC73C;.... display 11&#xC778; &#xACB0;&#xACFC;&#xBB3C;&#xC774; &#xC798; &#xCD9C;&#xB825;&#xB418;&#xC5C8;&#xB2E4;.](../.gitbook/assets/image%20%2829%29.png)

