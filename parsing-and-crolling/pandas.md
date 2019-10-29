# Pandas

```text
import pandas as pd
```

## List -&gt; Dataframe

> save\_result = \[ \(1,a\),\(2,b\),\(3,c\) \] 의 리스트형식이다  
> labels = \[ 'num', 'text' \]  로 인덱스를 지정함  
> dataframe 으로 형식을 변환함

{% embed url="https://pbpython.com/pandas-list-dict.html" %}

> 위의 글에 dict,list 등 다양한 형식을 dataframe으로 변환하는 예제가 작성되어있다.

## csv파일로 저장하기

> save\_result = \[ \(1,a\),\(2,b\),\(3,c\) \] 의 리스트형식이다  
> labels = \[ 'num', 'text' \]  로 인덱스를 지정함  
> dataframe 으로 형식을 변환함  
> to\_csv로 저장한다.  
> 한글이 깨지는 문제를 막기위해 encoding은 'euc-kr', 'cp949'를 사용하지만  
> 그럴 경우 특정 문자가 변환이 안되는 문제가 발생한다  
> 문제가 생기는 문자를 미리 제거해서 넣는 방법도 있지만  
> utf-8-sig 인코딩을 사용하면 문제가 발생하지않는



```text
df = pd.DataFrame.from_records(save_result, columns=labels)
df = df.applymap(lambda x: x.replace('\xa0','').replace('\xa9',''))
df.to_csv("navercafe_crawling.csv",encoding="utf-8-sig",header=False,index=False,mode='a')
```

