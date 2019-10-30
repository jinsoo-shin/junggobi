# Untitled



chmod 400 cert4.pem

ssh -i cert4.pem ubuntu@ip주소



sudo su \(관리자 권한\)



단 apt-get을 사용하여 최신 정보로 업데이트를 수행합니다.   
apt-get update

자바설치하기  
apt-get install default-jre





```text
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.1.0-linux-x86_64.tar.gz
tar -xzf elasticsearch-7.1.0-linux-x86_64.tar.gz
cd elasticsearch-7.1.0/ 
```





또 javapath 문제가 발생했다.

위치를 확인해보자

sudo update-alternatives --config java

여기에 설치되어있다

/usr/lib/jvm/java-11-openjdk-amd64/bin/java



