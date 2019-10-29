# 엘라스틱서치를 통해 검색을 사용해보자.

## 엘라스틱 서치를 사용해서 검색하고 데이터를 받아보자.

```text
@api_view(['GET', 'POST'])
def search(request):
    if request.method == 'GET':
        search_word = request.query_params.get('search_word')
        if search_word:
            es = Elasticsearch()
            docs = es.search(index='productinfo-index',
                             body={
                                 "size": 2,
                                 #지정안할시에 size는 기본적으로 10개이지만 테스트를 위해 2로했다.
                                 "from":0,
                                 "query": {
                                     "multi_match": {
                                         "query": search_word, #검색
                                         "fields": ["title", "contents", "region"]#어떤 필드에 적용할것인가?
                                     }
                                 },
                             })

            return Response(docs)
```

{% code-tabs %}
{% code-tabs-item title="결과물" %}
```text
"hits": {
        "total": {
            "value": 62,
            "relation": "eq"
        },
        "max_score": 3.0094876,
        "hits": [
            {
                "_index": "productinfo-index",
                "_type": "_doc",
                "_id": "108669009",
                "_score": 3.0094876,
                "_source": {
                    "category": "태블릿",
                    "manufacturer": "애플",
                    "model_name": "아이패드",
                    "cellular": "WIFI",
                    "price": 가,
                    "region": "지",
                    "date": "2019-10-28",
                    "link": "주소주",
                    "img_src": "이미.jpg",
                    "is_sell": 0,
                    "title": "제",
                    "contents: "내용내"
                },
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 집계 함수를 사용해보자.

{% code-tabs %}
{% code-tabs-item title="날짜별로 최저,최고,평균 가격을 뽑아보겠다." %}
```python
"aggs": {#aggregations                                     
    "group_by_date": {
         "terms": {
             "field": "date",
             "format": "yyyy-MM-dd" #포맷 지
         },
         "aggs": {
             "date_avg": {
                 "avg": {
                     "field": "price" #price 필드를 사용하겠
                 },
             },
             "date_max": {
                 "max": {
                     "field": "price"
                 }
             },
             "date_min": {
                 "min": {
                     "field": "price"
                 }
             }
         }
        },
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![](../.gitbook/assets/image%20%2823%29.png)

## 검색어 결과를 강조해보자!

```text
"highlight": {
     "pre_tags": ["<string>"],
     "post_tags": ["</string>"],
     "fields": {#어떤 필드를 강조할것인가??
         "title": {},
         "contents":{}
     },
 },
```

![&amp;lt;strong&amp;gt; &#xC544;&#xC774; &amp;lt;/string&amp;gt; ](../.gitbook/assets/image%20%2843%29.png)









