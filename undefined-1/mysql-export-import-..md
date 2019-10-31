# mysql 데이터 export/import하기.

> 넣으려고 하니깐 'utf8mb4\_0900\_ai\_ci' 가 에러가 발생하여 한글이 깨지는 현상이 생겼다.

![](../.gitbook/assets/image%20%284%29.png)

> 테이블 생성 부분을 수정해준다!

```text
ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci;
```

![](../.gitbook/assets/image%20%2812%29.png)

