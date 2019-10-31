# 성공코드.

![](../.gitbook/assets/image%20%2865%29.png)

```text
from selenium import webdriver
from bs4 import BeautifulSoup
import requests
import time
import urllib.request
import re

filename = 'navercafe_passtext.txt'
passtext=[]
with open(filename,mode='r',encoding='utf-8') as data:
    passtext = data.readlines()
    passtext = [x.strip('\n') for x in passtext]
    print(passtext)
    ##파일읽어와서 리스트로 만들었음
    
    

# driver = webdriver.Chrome("./chromedriver.exe")
# driver.implicitly_wait(3)
# # driver.implicitly_wait(3)
# driver.get('https://nid.naver.com/nidlogin.login')
# id = ''
# pw = ''
# driver.execute_script("document.getElementsByName('id')[0].value=\'" + id + "\'")
# driver.execute_script("document.getElementsByName('pw')[0].value=\'" + pw + "\'")
# driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()
#
# try :
#     driver.find_element_by_css_selector('.btn_upload > .btn') # 에러가 발생할 가능성이 있는 코드
#     driver.find_element_by_css_selector('.btn_upload > .btn').click()
# except:  # 에러 종류
#     pass #에러가 발생 했을 경우 처리할 코드
#
# try:
#     driver.find_element_by_css_selector('.btn_cancel3')
#     driver.find_element_by_css_selector('.btn_cancel3').click()
# except:
#     pass
#
# url="https://cafe.naver.com/ArticleSearchList.nhn?search.clubid=10050146&search.searchdate=all&search.searchBy=&search.query=%BE%C6%C0%CC%C6%D0%B5%E5&search.defaultValue=1&search.menuid=749"
# driver.get(url)
# driver.implicitly_wait(3)
# dom = BeautifulSoup(urllib.request.urlopen(url).read().decode("cp949",'ignore'), "html.parser")
# dom1 = dom.find_all(class_='td_article')
#
# passtext=['구매','삽니다','교환','매입','구합니다'] #제목이 이 단어를 포함하면 넘긴다!
#
# list = []
# cafeurl = "https://cafe.naver.com"
# for element in dom1:
#     a = element.find('a')
#     title = a.get_text(strip=True)
# #
#     if any(format in title for format in passtext):
#         pass # 만약 passtext의 문자열을 title이 포함하고 있다면 넘긴다!
#     else:
#         print(title)
#         element_url=cafeurl+a.get('href')
#         list.append(element_url)
# ################################################################## url 가져오는 파트

# url = list[0]
# print(url)

# url="https://cafe.naver.com/joonggonara?iframe_url=/ArticleRead.nhn%3Fclubid=10050146%26page=1%26menuid=749%26inCafeSearch=true%26searchBy=1%26query=%BE%C6%C0%CC%C6%D0%B5%E5%26includeAll=%26exclude=%26include=%26exact=%26searchdate=all%26media=0%26sortBy=date%26articleid=653257971%26referrerAllArticles=false"
#
# url="https://cafe.naver.com/joonggonara/653257971"
# response = requests.get(url)

# url="https://cafe.naver.com/ArticleRead.nhn?articleid=653260387&clubid=10050146"


url="https://cafe.naver.com/ArticleRead.nhn?clubid=10050146&page=4&menuid=749&inCafeSearch=true&searchBy=0&query=%BE%C6%C0%CC%C6%D0%B5%E5&includeAll=&exclude=&include=&exact=&searchdate=all&media=0&sortBy=date&articleid=653260387&referrerAllArticles=false"
url="https://cafe.naver.com/ArticleRead.nhn?clubid=10050146&page=1&menuid=749&inCafeSearch=true&searchBy=0&query=%BE%C6%C0%CC%C6%D0%B5%E5&includeAll=&exclude=&include=&exact=&searchdate=all&media=0&sortBy=date&articleid=653279531&referrerAllArticles=false"

# driver.get(url)
# driver.implicitly_wait(10)
# html = driver.page_source
# page = BeautifulSoup(html,'html.parser')
page = BeautifulSoup(urllib.request.urlopen(url).read().decode("cp949",'ignore'), "html.parser")
print(page)



# print(driver.find_element_by_class_name("main-area"))
# # print(soup.text)
# print(soup.find(class_="list-blog"))





# page = None
# while page is None:
#     try:
#         time.sleep(1)
#         page = BeautifulSoup(urllib.request.urlopen(url).read().decode("cp949",'ignore'), "html.parser")
#         print(page)
#     except:
#         print("pass")
#         continue
#
inbox = page.find(class_="inbox")
tt= page.find(class_=["tbody","m-col-c"])
print(tt.find(class_="image_condition").find("img").get('src'))
print(tt.find(class_="title").text)
print(tt.find(class_="cost").text)
print("#########################")
ttt = tt.find_all(['p','span']) #p랑 span을 포함하는 태그 불러옴
not_class_list=['txt','title','msg','m-tcol-c','note','cost','won','add_row','safety_deal_buy','safety_deal_help','sp_end']
list=[]
for t in ttt:
    try:
        class_ = t['class']
        # print(t.get_text())
        if any(c in class_ for c in not_class_list):
            continue
        else:
            list.append(t.get_text())
            # print(class_)
    except:
        # print(t.get_text())
        text = t.get_text()
        print(text)
        text = re.sub("[\xa0]", " ", text)
        if text in "상품 구매시 주의해 주세요!":
            continue
        list.append(text)

print(list)
#>>> head_tag = bs . find ( 'head' )
# >>> head_tag . find ( 'title' )                # head 태그 내부 title 태그의 내용을 불러온다
# >>> bs . find ( 'p' , align = "right" )   # p 태그 중  /and/   align = "right" 을 포함한 태그를 불러온다
# <p align="right....... >
# >>> body_tag = bs . find ( 'body' )
# >>> list1 = body_tag. find_all ( ['p','img'] )   # [] 리스트 , 'p'  /or/  'img' 를 포함하는 태그를 불러온다
# >>> for tag in list1 :
#      print ( tag )
# 출처: https://systemtrade.tistory.com/345 [시스템매매]
#     print("#####")
```

