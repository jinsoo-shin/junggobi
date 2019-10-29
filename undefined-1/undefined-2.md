# 노리 형태소 분석기를 써보자

## 노리 분석시 설치후...

> 설치를 했으니 이제 mapping부분에 analzer 부분에 노리를 추가해야한다.

```python
from elasticsearch_dsl.connections import connections
from elasticsearch_dsl import Document, Keyword, Text, Integer, Date,Search,tokenizer,analyzer,analysis,Boolean

from elasticsearch.helpers import bulk
from elasticsearch import Elasticsearch
from . import models
import json

connections.create_connection(hosts=['localhost'])
my_analyzer = analyzer(
    'my_analyzer',
    tokenizer=tokenizer('nori_tokenizer')
)
class ProductInfoIndex(Document):

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
    title = Text(analyzer=my_analyzer) #여기에 추
    contents = Text(analyzer=my_analyzer)
    class Index:
        name = 'productinfo-index'



def bulk_indexing():
    ProductInfoIndex.init()
    es = Elasticsearch()

    bulk(client=es, actions=(b.indexing() for b in models.ProductInfo.objects.all().iterator()))

```



