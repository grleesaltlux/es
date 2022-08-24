ElasticSearch 설치가이드
=============

## 준비
### 준비 및 파일 권한 수정
```shell
git clone https://github.com/grleesaltlux/es.git
cd es
mkdir data
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
    container_name: ann_es
    environment:
      - discovery.type=single-node # 싱글 es 설정
      #- bootstrap.memory_lock=true # 아래 옵션이랑 세트로 생략
      #- "ES_JAVA_OPTS=-Xms512m -Xmx512m" # min, max memory 설정 (생략가능)
    ports:
      - "9200:9200"
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
docker ps | grep ann_es
```
### 컨테이너 로그 확인
```shell
docker logs -f ann_es
```
### 컨테이너 삭제
```shell
docker-compose down
```
---
## 사용 방법
### 목차
- [인덱스 전체 조회](https://github.com/grleesaltlux/es#인덱스-전체-조회)
- [인덱스 생성](https://github.com/grleesaltlux/es#인덱스-생성)
- [인덱스 조회](https://github.com/grleesaltlux/es#인덱스-조회)
- [인덱스 삭제](https://github.com/grleesaltlux/es#인덱스-삭제)
- [doc 등록](https://github.com/grleesaltlux/es#document-등록)
### 인덱스 전체 조회
- method : GET
- url
  ```
  http://${IP}:${PORT}/_cat/indices?v
  ```
- shell
  ```
  curl ${IP}:${PORT}/_cat/indices?v
  ```
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

### 인덱스 조회
- method : GET
- Variable
  - INDEX_NAME : 생성하려는 인덱스 이름
- url
  ```
  http://${IP}:${PORT}/${INDEX_NAME}?pretty
  ```
- shell
  ```
  curl -X PUT ${IP}:${PORT}/${INDEX_NAME}?pretty
  ```

### 인덱스 삭제
- method : DELETE
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
  
### document 등록
- method : DELETE
- Variable
  - INDEX_NAME : document를 등록하려는 인덱스 이름
  - ID : document 번호
- header
  - Content-type:application/json
- shell
  ```
  curl -XPOST '${IP}:${PORT}/${INDEX_NAME}/_doc/${ID}?pretty' \
    -H 'Content-type:application/json' \
    -d '{"name": "test"}'
  ```
