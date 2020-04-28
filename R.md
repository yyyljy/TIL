# R

r-3.6.3

r studio

## 기초

- 변수에 값을 저장할 때는 <- (alt + -)기호를 사용하자

- 주석(#) 네개를 달면 책갈피 기능.

- | 종류      | n차원 | type                    |
  | --------- | ----- | :---------------------- |
  | Vector    | 1차원 | 동일                    |
  | Matrix    | 2차원 | 동일                    |
  | dataframe | 가변  | 열마다 다르게 가능      |
  | list      | 가변  | 단위(?)마다 다르게 가능 |

  

## 기본 함수

- class() : 타입 확인
- as.numeric() : 숫자로 형변환
- as.character() : 문자형으로 변환
- myvector <- c(100,200,300) : vector 생성
-  names(v1) <- c("국어","수학","영어","자바")
  - 벡터 요소에 이름 지정
- length(v1) : 길이
- mean(v1) : 평균
- max(v1) : 최대
- min(v1) : 최소
- mymat1 <- matrix(1:15,ncol=3,byrow = TRUE) : 배열 생성 및 옵션
- mymat1[1,c(1:3)]
- mymat1[1,]  : 1행의 모든 값
- mymat1[,1]  : 1열의 모든 값
- 랜덤함수
  - a <- sample(1:10, size = 5,replace = TRUE)
  - replace옵션 : 중복 허용

## 조건 선별

- myresult <- d[d$X3>=13,]

## 파일 입출력

- mdf <- read.csv("csv_exam.csv")

- write.csv(myresult,file="result.csv")

- head() 데이터 앞부분 일부만 읽어오기

- 미션

  ```
  csv_exam.csv를 읽어서 데이터를 수정한 후
  csv_exam_result.csv로 저장하기
  	science가 80이상인 데이터를 추출
  	추출된 데이터에 mytotal과 myavg컬럼을 추가
  	mytotal : 모든 과목의 총점
  	myavg : 모든 과목의 평균
  ```

- data.table

  ```
  dftable <- fread("encoding_test_utf8.csv")
  head(dftable)
  ```

- 인코딩

  ```
  Encoding(dftable$mydata3) <- "UTF-8"
  head(dftable)
  ```

- 읽기 쓰기

  ```
  ####파일write####
  write.csv(df,"encoding_test.csv",row.names = FALSE)
  write.csv(df,"encoding_test_euckr.csv",row.names = FALSE,fileEncoding = "euc-kr")
  write.csv(df,"encoding_test_cp949.csv",row.names = FALSE,fileEncoding = "cp949")
  write.csv(df,"encoding_test_utf8.csv",row.names = FALSE,fileEncoding = "utf8")
  
  ####깨진파일 처리하기####
  read.csv("encoding_test_utf8.csv")
  read
  readLines("encoding_test_utf8.csv")
  readLines("encoding_test_utf8.csv",encoding = "UTF-8")
  ```

- ## 사용자 정의 구분 기호

  ```
  #하나의 컬럼안에 구분자로 텍스트가 구성된 경우 분리
  install.packages("splitstackshape")
  library("splitstackshape")
  ```

- ## tsv

  ```
  ####tsv파일읽기####
  #tab으로 구분된 파일(\t)
  df3 <- read.delim(("02_tsv.txt"), sep = "\t")
  head(df3)
  ```

- ## xml

  ```
  install.packages("XML")
  library("XML")
  
  #xml에 있는 모든 태그의 name과 value를 추출
  #-XXXApply함수는 for문 대신 사용할 수 있는 함수
  dfxml <- xmlSApply(dfxml,function(x){
    xmlSApply(x,xmlValue)
  })
  dfxml
  ```

- ## xlsx

  ```
  install.packages("readxl")
  library("readxl")
  ```
  
- ## RData

  ```
  save(final_data,file = "crawl_data.RData")
  ```


# 데이터분석

- 데이터 가져오기

  - 외부파일
  - 크롤링
  - DB(오라클,몽고디비,하둡,...)
  - R에서 사용할 수 이쓴 여러 형태의 데이터로 변환, 변환된 데이터를 엑세스

- 데이터의 정보를 확인

  - 컬럼갯수, row갯수, 타입, 유형, 실제 저장된 데이터...

- 컬럼의 이름을 변경 - dplyr

  ```
  install.packages("dplyr")
  library("dplyr")
  #rename(data, 변경할 컬럼명=기존컬럼명)
  exam <- rename(exam, eng = english)
  exam
  ```

  # crawling -  basic

  ```
data(package="MASS") # R내부의 샘플데이터셋을 가져오기
  library(MASS)
  data("Boston")
  head(Boston)
  t(t(colnames(Boston)))
  df <- head(Boston[1:7])
  #반복작업을 할 때 사용할 수 있도록 함수를 지원
  #margin =1 => 행방향, 2: 열방향
  df[,"total"] <- apply(X = df,MARGIN = 1,FUN = "sum")
  df[,"avg"] <- apply(X = df,MARGIN = 1,FUN = "mean")
  df
  
  round(apply(X = df,MARGIN = 2,FUN = "sum"),4)
  round(apply(X = df,MARGIN = 2,FUN = "mean"),4)
  
  #sapply = apply의 margin속성을 2로 정의한 함수
  sapply(X = df,FUN = "mean")
  ```
  
  - 패턴 탐색
  
    ```
    install.packages("stringr")
    library("stringr")
    #패턴
    mytext <- "test$uuuuu"
    mytext2 <- "https://cran.r-project.org/"
    #패턴과 일치하는 문자열도 리턴
    str_extract(mytext2,".+(:)")
    ```
  
    - 전방 탐색 (?=)
  
      ```
      ##전방 탐색
      str_extract(mytext2,".+(?=:)")
      ```
  
    - 후방 탐색 (?<=)
  
      ```
      ##후방 탐색
      str_extract(mytext,"(?<=\\$).*")
      ```
  
      
  
  copy -> copy selector
  
  #div_content > div.list_content > div:nth-child(6) > div.list_title > a.list_subject > span.subject_fixed
  
  copy -> copy xpath
  
  //*[@id="div_content"]/div[6]/div[6]/div[2]/a[1]/span[2]

## crawling - N2H4

```
install.packages("N2H4")
library(N2H4)
library(stringr)
library(dplyr)
url <- "https://news.naver.com/main/read.nhn?mode=LSD&mid=shm&sid1=100&oid=020&aid=0003276790"
getAllComment(url) %>% 
  select(userName, contents) -> mydata  
mydata
class(mydata$contents)
```

## crawling - rvest

```
#css의 선택자를 활용할 수 있는 방법 - rvest
install.packages("rvest")
library(rvest)
url <- "https://www.clien.net/service/group/community?&od=T31&po=0"
readPage <- read_html(url)
readPage %>% 
  html_nodes("span.subject_fixed") %>% 
  html_text() -> title_data 
title_data
```



## MongoDB 연동

1. 연결

   ```
   con <- mongo(collection = "crawl", db = "bigdata", url = "mongodb://127.0.0.1")
   ```

2. 변환( data.frame 형식으로 변경)

   ```
   final_data <- data.frame(final_data)
   ```

3. insert

   ```
   #데이터가 있으면 삭제
   if(con$count()>0){
     con$drop()
   }
   #데이터 insert
   con$insert(final_data)
   ```

   