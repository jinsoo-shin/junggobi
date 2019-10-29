# elasticsearch 검색 기능

{% api-method method="get" host="http://localhost:8000/api/search" path="/?search\_word={string}" %}
{% api-method-summary %}
Get Data
{% endapi-method-summary %}

{% api-method-description %}
메인 페이지에서 검색할때 필요한 api.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="search\_word" type="string" required=false %}
검색하고자 하는 키워드를 입력해주세요.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "total": {
        "value": 14,
        "relation": "eq"
    },
    "max_score": 2.3973696,
    "hits": [
        {
            "_index": "productinfo-index",
            "_type": "_doc",
            "_id": "108669009",
            "_score": 2.3973696,
            "_source": {
                "category": "태블릿",
                "manufacturer": "애플",
                "model_name": "아이패드",
                "cellular": "WIFI",
                "price": 443000,
                "date": "2019-10-25",
                "link": "https://m.bunjang.co.kr/products/108669009?ref=검색결과&q=아이패드",
                "img_src": "https://seoul-p-studio.bunjang.net/product/108669009_1_1571934294_w292.jpg",
                "is_sell": 0,
                "title": "[아이패드미개봉] 이가격 실화~?",
                "contents": "1.아이패드미니5세대\r\n●아이패드미니5 WIFI 64GB 👉450000원\r\n●아이패드미니5 WIFI 256GB 👉630000원\r\n●아이패드미니5 [셀룰러] 64GB 👉600000원 \r\n●아이패드미니5 [셀룰러] 256GB 👉780000원\r\n\r\n2.아이패드에어3 \r\n●아이패드에어3 WIFI 64GB👉560000원\r\n●아이패드에어3 WIFI 64GB 👉725000원\r\n●아이패드에어3 [셀룰러] 64GB 👉710000원\r\n●아이패드에어3 [셀룰러] 256GB 👉860000원\r\n\r\n3.아이패드프로3세대 11형\r\n●아이패드프로3 11 WIFI 64GB👉862000원\r\n●아이패드프로3 11 WIFI 256GB👉1050000원\r\n●아이패드프로3 11 WIFI 512GB👉1280000원\r\n●아이패드프로3 11[셀룰러]64GB👉1060000원\r\n●아이패드프로3 11[셀룰러]256GB👉1250000원\r\n●아이패드프로3 11[셀룰러] 512GB 👉1520000원\r\n\r\n4.아이패드프로3 12.9형 \r\n●아이패드프로3 12.9 WIFI 64GB 👉1100000원\r\n●아이패드프로3 12.9 WIFI 256GB👉1270000원\r\n●아이패드프로3 12.9 WIFI 512GB👉1580000원\r\n●아이패드프로3 12.9 [셀룰러] 64GB👉1270000원\r\n●아이패드프로3 12.9 [셀룰러] 256GB👉1450000원\r\n●아이패드프로3 12.9 [셀룰러] 512GB👉 재고없음\r\n\r\n✔️단가는 상시 변동 됩니다.\r\n✔️추가품으로 애플펜슬 구매 가능합니다.\r\n1세대: 107000원, 2세대 140000원\r\n✔️모두 미개봉 애플코리아 정품입니다.\r\n✔️가품일시 500%환불 해드립니다.\r\n✔️상세스펙은 카페 아이패드메뉴에서 확인해주세요\r\nhttp://cafe.naver.com/sosimple90\r\n✔️에눌,교신 문의X\r\n✔️셀룰러 모델은 자급제 공기계입니다.\r\n✔️문의 010-6313-9634"
            }
        },
        {
            "_index": "productinfo-index",
            "_type": "_doc",
            "_id": "110505788",
            "_score": 1.9996241,
            "_source": {
                "category": "태블릿",
                "manufacturer": "애플",
                "model_name": "아이패드",
                "cellular": "WIFI",
                "price": 20000,
                "region": "전국",
                "date": "2019-10-24",
                "link": "https://m.bunjang.co.kr/products/110505788?ref=검색결과&q=아이패드",
                "img_src": "https://seoul-p-studio.bunjang.net/product/110505788_1_1571895462_w292.jpg",
                "is_sell": 0,
                "title": "[30일 상점] ipad POUCH",
                "contents": "30일상점 아이패드 파우치입니다 ! \n\n한번 사용했으며 아이패드 프로 들고다니며 사용했을때 \n\n펜슬까지 넉넉하게 들어갔었습니다\n\n다른파우치들과는 다르게 안감이 부들부들하여 \n\n특유의 본드냄새도 나지 않고 \n\n소중한 아이패드를 귀엽게 보호 하실수있어요 ! \n\n택배비 3000 원 추가해주세요😊\n\n마지막 사진이 실제 사진입니다!"
            }
        },
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a cake matching this query.
{% endapi-method-response-example-description %}

```javascript
{
    "message": "Ain't no cake like that."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="http://localhost:8000/api/search" path="/?search={string}" %}
{% api-method-summary %}
Index  초기
{% endapi-method-summary %}

{% api-method-description %}
{string}에 아무거나 넣어도 작동합니다.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="search" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

