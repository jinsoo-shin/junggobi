# AWS에 설치하기



chmod 400 cert.pem

$ ssh -i cert.pem ubuntu@ip주소



sudo su \(관리자 권한\)



단 apt-get을 사용하여 최신 정보로 업데이트를 수행합니다.   
apt-get update

자바설치하기  
apt-get install default-jre

  sudo apt-get install openjdk-8-jre  

sudo apt-get install openjdk-8-jdk  


## JAVA\_HOME 환경변수 설정

![](../.gitbook/assets/image%20%288%29.png)



위치

![](../.gitbook/assets/image%20%2855%29.png)

![](../.gitbook/assets/image%20%2812%29.png)

위치 확인

/usr/lib/jvm/java-8-openjdk-amd64/

{% code title="프로필 열기" %}
```text
sudo nano /etc/profile
```
{% endcode %}

{% code title="밑에 추가하기" %}
```text
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$JAVA_HOME/bin/:$PATH
export CLASS_PATH=$JAVA_HOME/lib:$CLASS_PATH

```
{% endcode %}

> 저장하기 ctrl+x  -&gt;  y  -&gt; enter

{% code title="프로필 리로드!" %}
```text
source /etc/profile 
```
{% endcode %}

{% code title="서버 재시작 안해도 인식하기는 함" %}
```text
sudo reboot now

```
{% endcode %}

{% code title="위치확인" %}
```text
 echo $JAVA_HOME

```
{% endcode %}

![](../.gitbook/assets/image%20%2859%29.png)

## 실행.

{% code title="명령어들" %}
```text
sudo service elasticsearch restart
sudo service elasticsearch start
sudo -i service elasticsearch start
sudo -i service elasticsearch stop
```
{% endcode %}

> 백그라운드에서 실행이 되는지 안보여서 curl로 실행해보았다.

```text
 curl GET '127.0.0.1:9200'
```

![ curl GET &apos;127.0.0.1:9200&apos; &#xD655;&#xC778;](../.gitbook/assets/image%20%2820%29.png)



![](../.gitbook/assets/image.png)





> /usr/share/elasticsearch\# sudo bin/elasticsearch-plugin install analysis-nori

```text
sudo bin/elasticsearch-plugin install analysis-nori
```

![&#xC124;&#xCE58;&#xC644;&#xB8CC;&#xD588;&#xB2E4;&#x3160;&#x3160;](../.gitbook/assets/image%20%2852%29.png)

![&#xC124;&#xCE58; &#xD655;&#xC778;!!!!](../.gitbook/assets/image%20%2854%29.png)

## 외부접속 열어주자!

> elasticsearch.yml 설정을 수정하자

파일은  /etc/elasticsearch에 위치하고있다.

![](../.gitbook/assets/image%20%2861%29.png)

![network.host&#xB97C; &#xACE0;&#xCE58;&#xACA0;&#xB2E4;](../.gitbook/assets/image%20%2831%29.png)

![&#xC218;&#xC815;](../.gitbook/assets/image%20%2825%29.png)

> 재시작을 해주자

```text
sudo service elasticsearch restart
```



 \) **curl: \(7\) Failed to connect to HOST\_NAME port 9200: 연결이 거부됨** 발생 시,  
- `sudo ufw allow 9200` 명령어로 9200번 포트를 열어준다.

## ssh 접속이 끊겨도 돌아가게 해주자!

![](../.gitbook/assets/image%20%2829%29.png)

> sudo nohup \[ 명령어 \] &

```text
sudo nohup service elasticsearch start &
```

![&#xCA14;&#xC794;](../.gitbook/assets/image%20%2834%29.png)

















```text
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.1.0-linux-x86_64.tar.gz
tar -xzf elasticsearch-7.1.0-linux-x86_64.tar.gz
cd elasticsearch-7.1.0/ 
```

