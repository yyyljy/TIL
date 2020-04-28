# Mongo DB

- NoSQL
- node.js와 연계해서 다양한 서비스 개발
- 비정형데이터(스키마가 없다)
- JSON
- join 불가



## 서버

- mongod -dbpath c:\bigdata\mongo
- 명령 프롬프트 클릭하거나 드래그 하지 않기.

## 클라이언트

- mongo
- show dbs

## 서비스로 등록

https://docs.mongodb.com/v3.6/tutorial/install-mongodb-enterprise-on-windows/#start-mongodb-enterprise-edition-as-a-windows-service

## 용어

collection ~= RDBMS 테이블

- 관계형 데이터베이스처럼 스키마를 정의하지 않는다.

- 종류

  - capped collection
    - 고정 사이즈를 주고 생성하는 컬렉션( ex. 보관 기간 3년 사이즈 계산 후 지정)
    - 미리 지정한 저장공간이 모두 사용되면 맨 처음에 저장된 데이터가 삭제되고 공간으로 활용
  - non capped collection
    - 일반적인 컬렉션

- 생성

  - db.createCollection("컬렉션명") - >일반 collection
  - db.createCollecton("컬렉션명",{옵션list})
    - 각각의 옵션을 설정해서 작업(json 형식)

- 삭제

  - db.collection명.drop()

- collections명 변경

  - db.collection명.renameCollection("변경할 이름")

- mongodb에 insert

  - db.collection명.insert({데이터..})
  - db.collection명.insertOne({데이터...})
  - db.collection명.insertMany({데이터...})
    - document(관계형db에서 레코드 개념)

- mongodb에 update

  - document수정

  - 조건을 적용해서 수정하기 위한 코드도 json으로 구현

  - $set :

  - ```
    db.collection명.update({조건필드,값},//sql의 update문 where절)
    	{$set:{수정할 필드:수정값}},//set절
     	{update와 관련된 옵션:옵션값}
    ```

  - multi = true 옵션을 추가하지 않으면 조건에 만족하는 document 중 첫 번째 document만 update

    ```
    db.emp.update({id:"kim"},{$set:{val2:4000}},{multi:true})
    ```

  - $inc : 해당 필드에 저장된 숫자 값을 증가시킴

    ```
    db.emp.update({id:"k"})
    ```

  - $unset : 해당 필드를 삭제

    ```
    db.score.update({id:"hong"},{$unset:{msg:"test"}})
    ```

- mongodb에서 배열 관리

  - db.score.update({id:"jang"},{$set:{info:{city:["서울","안양"], movie:["겨울왕국2","극한직업","쉬리"]}}})

  - addToSet : 배열에 값이 없을 경우 추가(중복 체크함)

    ```
    db.score.update({id:"jang"},{$addToSet:{"info.city":"인천"}})
    ```

  - push : 배열에 요소 추가 (중복 가능)

    ```
    db.score.update({id:"jang"},{$push:{"info.city":"천안"}})
    ```

  - pop : 배열에서 요소 제거 (1 : 마지막 제거, -1 : 첫번째 제거)

    ```
    db.score.update({id:"jang"},{$pop:{"info.city":1}})
    db.score.update({id:"jang"},{$pop:{"info.city":-1}})
    ```

  - each : addToSet이나 push에서 사용할 수 있다.

    - 여러개를 배열에 추가할 때 사용

    ```
    db.score.update({id:"jang"},{$push:{"info.city":{$each:["천안","가평","군산"]}}})
    ```

  - sort : 정렬(1:오름차순, -1:내림차순)

    ```
    db.score.update({id:"jang"},{$push:{"info.city":{$each:["천안","가평","군산"],$sort:1}}})
    ```

  - pull : 배열에서 조건에 만족하는 요소를 제거

    ```
    db.score.update({id:"jang"},{$pull:{"info.city":"천안"}})
    db.score.update({id:"jang"},{$pullAll:{"info.city":["가평","군산"]}})
    ```

    

document : 레코드

field : 컬럼

_id : 기본키

- document를 삽입하면 자동으로 생성

  - ```
    MongoDB Enterprise > db.emp.find()
    { "_id" : ObjectId("5e6ee794b0288e2a1a5002ba"), "id" : "jang", "pass" : "1234" }
    { "_id" : ObjectId("5e6ee7f5b0288e2a1a5002bb"), "id" : "lee", "pass" : "1234", "info" : "jjang" }
    ```

  - 현재 timestamp + machine id + mongodb프로세스id + 순차번호
  
- 기본 명령어

  ```
  document 보기 좋게 출력하기
  db.collection명.find().pretty()
  ```

- 모든 document에 field추가하기

  ```
  var x = db.score.find()
  while(x.hasNext()){
  	var one = x.next();
  	one.num = 1000;
  	db.score.save(one);
  }
  ```

- find()

  ```
  db.collection명.find(조건,조회할 필드에 대한 명시)
  db.score.find({addr:"인천"},{id:1,name:1,dept:1,addr:1})
db.score.findOne
  정규 표현식 적용 가능
db.collection명.find({조건필드명:/정규표현식/옵션})
  db.score.find({id:/kim|park/})
i옵션 : 대소문자 구분 없이 조회 가능
  db.score.find({id:/kim|park/i})
  ```
```
  
- db.collection명.find({})와 동일
  
- 조건, 조회할 필드에 대한 명시 모두 json
  
- 조회할 필드의 정보 명시
  
  - 형식:{필드명:1...} : 화면에 표시하고 싶은 필드
  
    - {필드명:0} : 명시한 필드가 조회되지 않도록 처리

  - 조건

    - $lt
  
    - $gt

    - $lte
  
    - $gte
  
```
      java점수가 90 이상
      db.score.find({java:{$gte:90}},{id:1,name:1,dept:1,java:1})
      ```
      
    - $or : 여러 필드를 이용해서 같이 비교
      
      ```
      dept가 인사이거나 addr이 인천
      db.score.find({$or:[{dept:"인사"},{addr:"인천"}]})
      id가 song or kong or hong
      db.score.find({$or:[{id:"song"},{id:"hong"},{id:"kang"}]})
      ```
      
    - $and
      
    - $in
      
      ```
      id가 hong or song or kang
      db.score.find({id:{$in:["hong","song","kang"]}})
      ```
      
    - $nin : not in
      
      ```
      id가 hong or song or kang 이 아닌 데이터
      db.score.find({id:{$nin:["hong","song","kang"]}})
      ```

  - count() : 행의 갯수를 리턴
  
  - limit(숫자) : 숫자만큼의 document만 조회
  
  - skip(숫자) : 숫자만큼의 document를 skip하고 조회
  
  - remove()
  
    - 조건 정의는 find(), update()와 동일
    
    ```
    db.score.remove({servlet:{$lt:80}})
    ```
  
    
  
    ```
    1번
    db.score.find("",{id:1,servlet:1})
    2번
    db.score.find({java:{$gte:70}})
    3번
    db.score.find({bonus:{$gte:2000}},{name:1,java:1})
    4번
    db.score.find({$and:[{dept:"인사"},{addr:{$in:["안양","대구"]}}]})
    5번
    db.score.find({$and:[{servlet:{$gte:70}},{servlet:{$lte:90}},{dept:"총무"}]})
    6번
    db.score.find({name:/^김/})
    7번
    db.score.find({servlet:{$nin:[null]}}).sort({servlet:1}).next()
    db.score.find({servlet:{$nin:[null]}}).sort({servlet:-1}).next()
    db.score.find({servlet:{$not:{$exists:null}}}).sort({servlet:1}).limit(1)
    db.score.find({servlet:{$not:{$exists:null}}}).sort({servlet:-1}).limit(1)
    8번
    db.score.find().sort({java:-1}).skip(2).limit(7)
    9번
    db.score.find({id:/n|o/})
    ```
  
- Aggregation

  - group by와 동일개념

  - 간단한 집계를 구하는 경우 mapreduce를 적용하는 것 보다 간단하게 작업

  - pipeline을 내부에서 구현

  - 한 연산의 결과가 또 다른 연산의 input데이터로 활용

  - https://docs.mongodb.com/v3.6/_images/aggregation-pipeline.bakedsvg.svg

  - 명령어

    - $match : where절, having절
    - $group : group by
    - $sort : order by
    - $avg : avg 그룹함수
    - $sum : sum 그룹함수
    - $max : max 그룹함수

  - 형식

    - db.collection명.aggregate(aggregate명령어를 정의(여러가지 정의 할 경우 배열 사용))
    - $group:{_id:그룹으로 표시할 필드명, 연산결과를 저장할 필드명:값}

    ```
    db.exam.aggregate([{$group:{_id:"$addr",num:{$sum:1}}}])
    db.exam.aggregate([{$group:{_id:"$dept",num:{$avg:"$java"}}}])
    ```

    - $match:{필드명:{연산자:조건값}}

    ```
    db.exam.aggregate([{$match:{addr:"인천"}},{$group:{_id:"$dept",평균:{$avg:"$java"}}}])
    ```

    ```
    1번
    db.exam.aggregate([{$match:{dept:"인사"}},{$group:{_id:"$dept",평균:{$avg:"$servlet"}}}])
    2번
    db.exam.aggregate([{$match:{java:{$gt:80}}},{$group:{_id:"$dept",인원:{$sum:1}}}])
    3번
    db.exam.aggregate([{$match:{java:{$gt:80}}},{$group:{_id:"$dept",인원:{$sum:1}}},{$sort:{"인원":-1}}])
    ```

- Spring

  ![image-20200317171153366](C:\Users\student\Desktop\TIL\img\Mongo DB\image-20200317171153366.png)
  
- export

  ```
  mongoexport -d bigdata -c score -o score.json
  ```

- import

  ```
  mongoimport /d bigdata /c test /type csv /file test.csv /headerline
  ```

  - /headerline : 첫 줄 건너뜀
  - /h, /host:<hostname> /port:<port> : 원격 DB일 경우

