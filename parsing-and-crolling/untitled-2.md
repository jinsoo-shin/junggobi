# Untitled



```text
from selenium import webdriver
from bs4 import BeautifulSoup
import requests
import time
import urllib.request

# driver = webdriver.Chrome("./chromedriver.exe")
# driver.implicitly_wait(3)
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
# driver.get('https://cafe.naver.com/ArticleSearchList.nhn?search.clubid=10050146&search.searchdate=all&search.searchBy=&search.query=%BE%C6%C0%CC%C6%D0%B5%E5&search.defaultValue=1&search.menuid=749')
# driver.implicitly_wait(3)
# url="https://cafe.naver.com/ArticleSearchList.nhn?search.clubid=10050146&search.searchdate=all&search.searchBy=&search.query=%BE%C6%C0%CC%C6%D0%B5%E5&search.defaultValue=1&search.menuid=749"
# dom = BeautifulSoup(urllib.request.urlopen(url).read().decode("cp949",'ignore'), "html.parser")
# dom1 = dom.find_all(class_='td_article')
#
# passtext=['구매','삽니다','교환','매입'] #제목이 이 단어를 포함하면 넘긴다!
#
# list = []
# cafeurl = "https://cafe.naver.com"
# for element in dom1:
#     a = element.find('a')
#     title = a.get_text(strip=True)
#
#     if any(format in title for format in passtext):
#         pass # 만약 passtext의 문자열을 title이 포함하고 있다면 넘긴다!
#     else:
#         print(title)
#         element_url=cafeurl+a.get('href')
#         list.append(element_url)
################################################################## url 가져오는 파트

# url = list[0]
# print(url)

url = "https://cafe.naver.com/joonggonara?iframe_url=/ArticleRead.nhn%3Fclubid=10050146%26page=1%26menuid=749%26inCafeSearch=true%26searchBy=0%26query=%BE%C6%C0%CC%C6%D0%B5%E5%26includeAll=%26exclude=%26include=%26exact=%26searchdate=all%26media=0%26sortBy=date%26articleid=653099967%26referrerAllArticles=false"

response = requests.get(url)
dom = BeautifulSoup(urllib.request.urlopen(url).read().decode("cp949",'ignore'), "html.parser")
print(dom)



# driver.get(url)
# driver.implicitly_wait(3)
# print(driver.find_element_by_class_name("main-area"))
# html = driver.page_source
# soup = BeautifulSoup(html,'html.parser')
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

# inbox = page.find(class_="inbox")
# tt= page.find(class_=["tbody","m-col-c"])
# print(tt.find(class_="image_condition").find("img").get('src'))
# print(tt.find(class_="title").text)
# print(tt.find(class_="cost").text)
# print("#########################")
# ttt = tt.find_all("p")
# for t in ttt:
#     print(t)
#     print("#####")
```

