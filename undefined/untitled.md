# Untitled

## 1일차

> django & mysql 연동을 시도하였으나 실패하였다 \(이유는 복붙하면서 글자가 깨진듯\(?\)\)

> ```text
> df = df.applymap(lambda x: x.replace('\xa0','').replace('\xa9',''))
> ```
>
> ```text
> #-*- coding:utf-8 -*-
> ```
>
> 위의 코드로 인코딩 문제를 해결하였다.
>
> navercafe 크롤링을 시작하였다
>
> 게시판 메인 긁어오기 성공.

## 2일차

> django와 mysql 연동 및 gitbook에 작성 끝
>
> navercafe크롤링 title,가격 성공하고 &lt;p&gt;로 된 내용을 긁다가 알 수 없는 에러로 멈춘 상태.
>
> =&gt; 이 에러는 인코딩 문제였다. cp949에서 utf-8로 읽어오면서 변환이 안되는 '\xa0'과 같은 글자들을 못 읽어오면서 생긴 문제였다.

## 3일차 10/16

> navercafe text까지 긁어오기 성공하였다
>
> mysql에 테이블을 생성하고 테이블을 Django에 적용시켰다.

## 4일차 10/17

> 오전 중에는 심화프로젝트 발표때문에 진행을 못함.
>
> 일단 나중에 DB에 넣을 것을 생각하여 미리 csv에 저장하고있다.

## 2주 1일차 10/21

> 하루종일 네이버 카페 크롤링한 결과물 전처리에 고통을 겪음
>
> 가격부분도 처리했다
>
> db를 설계하는데 생각보다 url이 길었

## 2주 2일차 10/22

> db를 다시 설계하는 부분에서 엘라스틱서치를 사용하기위해 title과 contents를 추가하였다
>
> 고로 오늘은 엘라스틱서치 공부할거



{% embed url="https://blog.nerdfactory.ai/2019/04/29/django-elasticsearch-restframework.html" %}











