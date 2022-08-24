Step 01. Create AWS IAM
=============

## 준비
### git clone
```shell
git clone https://github.com/grleesaltlux/es.git
```
### 파일 권한 수정
```shell
chmod 777 ./data
chmod 755 ./elasticsearch.yml
```
### docker-compose.yaml
```
version: '3.8'

networks:
ann_es:
driver: bridge

services:
es:
image: elasticsearch:8.1.2 # 사용 이미지
environment:
- discovery.type=single-node # 싱글 es 설정
- bootstrap.memory_lock=true
- "ES_JAVA_OPTS=-Xms512m -Xmx512m" # min, max memory 설정
ports:
- "9200:9200"
- "9300:9300"
volumes:
- ./data:/usr/share/elasticsearch/data # data mount
- ./elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml # 설정 파일
networks:
- ann_es
```
---
## 실행

### use docker compose
```shell
docker-compose up -d
```
### 컨테이너 확인
```shell
docker ps | grep elasticsearch_es_1
```
### 컨테이너 삭제
```shell
docker-compose down
```
---
## 사용

### 인덱스 전체 조회
- method : GET
- url
  ```http request
  http://${IP}:${PORT}/_cat/indices?v
  ```
- shell
  ```
  curl ${IP}:${PORT}/_cat/indices?v
  ```
- output
 

    health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
    yellow open   test  HTmfuNd3R2ec-SD4LzctyA   1   1          0            0       225b           225b
### 인덱스 생성
- method : PUT
- Variable
  - INDEX_NAME : 생성하려는 인덱스 이름
- url
  ```
  http://${IP}:${PORT}/${INDEX_NAME}
  ```
- shell
  ```
  curl -X PUT ${IP}:${PORT}/${INDEX_NAME}
  ```
- output sample


    {"acknowledged":true,"shards_acknowledged":true,"index":"test"}

---
### reference
- https://hub.docker.com/_/elasticsearch
- https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

 
http://211.109.9.144:9200/_aliases?pretty=true

_status

클러스터에 존재하는 인덱스 전체 조회
http://211.109.9.144:9200/_cat/indices?v

인덱스 생성
PUT /test?pretty
인덱스 조회
GET /test?pretty
인덱스 삭제
DELETE /test?pretty

### 1. create IAM user using AWS console
- user access type
    - Access Key
    - Password
- Permissions policies
    - AdministratorAccess
    - AmazonEKSVPCResourceController

### 2. AWS Configure IAM to server
```shell
aws configure
```
>AWS Access Key ID [****************AXNL]: ${public access key}
>
>AWS Secret Access Key [****************UIqm]: ${private access key}
>
>Default region name [ap-northeast-1]: ${region}
>
>Default output format [json]: json


- asd
  - https://hub.docker.com/_/elasticsearch
  - https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html