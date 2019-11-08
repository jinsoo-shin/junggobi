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

{% tabs %}
{% tab title="결과물" %}
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
{% endtab %}
{% endtabs %}

## 집계 함수를 사용해보자.

{% tabs %}
{% tab title="날짜별로 최저,최고,평균 가격을 뽑아보겠다." %}
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
{% endtab %}
{% endtabs %}

![](../.gitbook/assets/image%20%2837%29.png)

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

![&amp;lt;strong&amp;gt; &#xC544;&#xC774; &amp;lt;/string&amp;gt; ](../.gitbook/assets/image%20%2867%29.png)



## 합본.

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
                     "from":0,
                     "query": {
                         "multi_match": {
                             "query": search_word,
                             "fields": ["title", "contents", "region"]
                         }
                     },
                     "highlight": {
                         "pre_tags": ["<string>"],
                         "post_tags": ["</string>"],
                         "fields": {
                             "title": {},
                             "contents":{}
                         },
                     },
                     "aggs": {#aggregations
                         "avg_price": {
                             "avg": {
                                 "field": "price"
                             }
                         },
                         "max_price": {
                             "max": {
                                 "field": "price"
                             }
                         },
                         "min_price": {
                             "min": {
                                 "field": "price"
                             }
                         },
                         "group_by_date": {
                             "terms": {
                                 "field": "date",
                                 "format": "yyyy-MM-dd"
                             },
                             "aggs": {
                                 "date_avg": {
                                     "avg": {
                                         "field": "price"
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
                     }
                 })

            return Response(docs)
```

## 집계함수 bucket sort

>

{% tabs %}
{% tab title="내림차순으로 date\_avg를 정렬하겠다. size도 추후에 추가가능!" %}
```text
"aggs": {
         "date_avg": {
             "avg": {
                 "field": "price"
             },
         },
          "date_bucket_sort": {
             "bucket_sort": {
                 "sort": [
                     {"date_avg": {"order": "desc"}},
                 ],
                 # "size": 7
             }
            }
     }
```
{% endtab %}
{% endtabs %}

>

![date\_avg&#xAC00; &#xB0B4;&#xB9BC;&#xCC28;&#xC21C;&#xC73C;&#xB85C; &#xC815;&#xB82C;&#xB418;&#xC5C8;&#xB2E4;.](../.gitbook/assets/image%20%2847%29.png)

## 집계함수 key 기준 sort

> 방금 전에는 결과 값인 bucket를 기준으로 sort를 했다면 이번엔 기준인 key를 기준으로 정렬을 해보겠다.

```text
"group_by_date": {
     "terms": {
         "field": "date",
         "format": "yyyy-MM-dd",
         "order": {"_key": "asc"} # terms에 추가한다 key 기준으로 오름차
          #"order" : { "_count" : "asc" },
          "size": 7 #일주일의 데이터를 보여주기위해 size 7
     },
     "aggs": {
         "date_avg": {
             "avg": {
                 "field": "price"
             },
         },
```

![](../.gitbook/assets/image%20%2866%29.png)

{% embed url="https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket-terms-aggregation.html" %}

## 멀티쿼

{% embed url="https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-multi-match-query.html" %}

![](../.gitbook/assets/image%20%2842%29.png)

```text
 "query": {
     # "multi_match": {
     #     "query": search_word,
     #     "fields": ["title", "contents", "region"]
     # },
     "bool": {
         "should": [
             {"match": {"title": search_word}},
             {"match": {"contents": search_word}},
             {"match": {"region": search_word}},
             {"match": {"manufacturer": ""}},
             {"match": {"model_name": "갤럭시"}}
         ]
     }
 },
```

