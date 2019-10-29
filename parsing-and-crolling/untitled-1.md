# Untitled



`id="Grid"`를 `{"id": "Grid"}`로 바꿔보세요.

```text
ranks = soup.findAll("div", {"id":"Grid"}) 
```

div 안에 있는 요소만 긁어 오시고 싶으시다면,

```text
ranks = soup.findAll("div", {"id":"Grid"}).findChildren()
```

