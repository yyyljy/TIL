# Flume - http://flume.apache.org/

- 데이터를 추출하기 위해 사용하는 프로그램
- 시스템로그, 웹 서버의 로그, 클릭로그, 보안로그.. 비정형데이터를 HDFS에 적재하기 위해 사용하는 프로그램
- 대규모의 로그데이터가 발생하면 효율적으로 수집하고 저장하기 위해 관리
- flume, chukwa, scribe, fluentd, splunk

![image-20200313101333395](C:\iot\img\image-20200313101333395.png)

![image-20200313102150946](C:\iot\Flume\image-20200313102150946.png)

- 여러 Flume서버에서 데이터를 수집 후 한 곳(그림 상 Agent4)에 통합

## Source

- 파일의 출처(?)
- File, Console,Sink, 네트워크 등을 통해 데이터를 수집

## Channel

```
Flume Channels
Channels are the repositories where the events are staged on a agent. Source adds the events and Sink removes it.
```

- Sink로 전달하기 전에 데이터를 모아놓는 곳(?)

## Sink

```
HDFS Sink
This sink writes events into the Hadoop Distributed File System (HDFS). It currently supports creating text and sequence files. It supports compression in both file types. The files can be rolled (close current file and create a new one) periodically based on the elapsed time or size of data or number of events. It also buckets/partitions data by attributes like timestamp or machine where the event originated. The HDFS directory path may contain formatting escape sequences that will replaced by the HDFS sink to generate a directory/file name to store the events. Using this sink requires hadoop to be installed so that Flume can use the Hadoop jars to communicate with the HDFS cluster. Note that a version of Hadoop that supports the sync() call is required.
```

- File, Console, HDFS or 다른 시스템의 Source로 전달
- Output을 담당하는 객체

## 설치 및 세팅

- 1. 다운로드

     wget http://archive.apache.org/dist/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz

  2. 압축 해제

     tar -zxvf apache-flume-1.6.0-bin.tar.gz

  3. .bashrc 에 환경변수 설정

     ![image-20200313104459237](C:\iot\Flume\image-20200313104459237.png)

  4. flume-env.sh rename하고 정보등록

     ![image-20200313111329521](C:\iot\Flume\image-20200313111329521.png)

  5. flume-conf.properties.template 을 rename (console.properties)

     - flume agent의 source, channel, sink에 대한 정보를 등록

       ```
       #agent
       myConsole.source=mySrc
       myConsole.channels=memChannel
       myConsole.sinks=mySink
       
       #channel
       myConsole.sources.mySrc.channels=memChannel
       myConsole.sinks.mySink.channel=memChannel
       
       #source
       myConsole.sources.mySrc.type=netcat
       myConsole.sources.mySrc.bind=localhost
       myConsole.sources.mySrc.port=44444
       
       #sink
       myConsole.sinks.mySink.type=logger
       
       #channel
       myConsole.channels.memChannel.type=memory
       myConsole.channels.memChannel.capacity=1000
       ```

     - source - 데이터가 유입되는 지정(어떤 방식으로 데이터가 유입되는지 type으로 명시)

     - channel - 데이터를 보관하는 곳(source와 sink사이의 Queue)

     - sink - 데이터를 내보내는 곳(어떤 방식으로 내보낼지 정의)

       - type
         - logger : flume서버 콘솔에 출력이 전달
           - flume을 실행할 때 -Dflume.root.logger=INFO,console를 추가
         - file_roll : file을 읽어서 가져오는 경우
           - (directory : 읽어온 파일을 저장할 output폴더를 명시)

  6. Flume의 실행

     ```
     ./bin/flume-ng agent --conf conf/ --conf-file ./conf/console.properties --name myConsole -Dflume.root.logger=INFO,console
     ```

     - 실행명령어 ./bin/flume-ng agent
     - 옵션
       - --conf : 설정파일이 저장된 폴더명(-c)
       - --conf-file : 설정파일명(-f)
       - --name : agent의 이름(-n)
     - -Dflume.root.logger=INFO,console : flume의 로그창에 기록

  7. Flume의 구성 요소

     - 실행 중인 프로세를 agent라 부르며 sourct, html, sonk





```
./bin/flume-ng agent -c conf -f ./conf/flume.conf --name myavro
```

