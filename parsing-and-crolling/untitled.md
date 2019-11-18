# 셀레니움 네이버 로그인

{% code title="네이버 로그인" %}
```text
from selenium import webdriver
from bs4 import BeautifulSoup
import requests
import urllib.request

driver = webdriver.Chrome("./chromedriver.exe")
driver.implicitly_wait(3)
driver.get('https://nid.naver.com/nidlogin.login')
id = '아이디.' #아이디.
pw = '비번.' #비밀번호.
driver.execute_script("document.getElementsByName('id')[0].value=\'" + id + "\'")
driver.execute_script("document.getElementsByName('pw')[0].value=\'" + pw + "\'")
driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()
```
{% endcode %}

