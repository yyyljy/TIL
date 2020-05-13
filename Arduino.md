# 아두이노

## 핀 세팅

```c
int pin = A2;
//A2핀
void setup(){}
	pinMode(pin, OUTPUT);
}

void loop(){
    digitalWrite(pin,HIGH);
    delay(1000);
    digitalWrite(pin,LOW);
    delay(1000);
}
```



## 배열로 LED 다루기

```c
int ledPins = {A2,A1};
int ledLength = sizeof((ledPins)/sizeof(int));
void setup(){
    for(int i=0;i<ledLength;i++){
        pinMode(ledPins[i],OUTPUT);
    }
}

void loop(){
    for(int i=0;i<ledLength;i++){
        digitalWrite(ledPins[i],HIGH);
        delay(1000);
        digitalWrite(ledPins[i],LOW);
        delay(1000);
    }
}
```





[시리얼 통신]

1. CommPortIndentifier를 포트의 유효성과 통신가능 상태인지 점검

2. commPortIdentifier의 open메소드를 이용해서 시리얼 통신을 할수 있는 주니밧앹로 셋팅

   => 시리얼통신을 하기 위해 필요한 포트객체가 리턴

3. CommPort는 종류가 2가지

   - Serial
   - Parallel

   CAN통신은 Serial통신, 아두이노와 라떼판다도 Serial통신

    => 각 상황에 맞는 CommPort객체를 얻어야 작업할 수 있다.

4. CommPort를 SerialPort로 casting

5. SerialPort객체의 setSerialPortParams메소드를 이용해서 Serial통신을 위한 기본내용을 설정

   SerialPort.setSerialPortParams(9600,	--> Serial port의 통신속도

   ​		SerialPort.DATABITS_8,					--> 전송하는 데이터의 길이

   ​		SerialPort.STOPBITS_1,					--> stop bit에 설정

   ​		SerialPort.PARITY_NONE);				--> PARITY비트(오류 검증 비트) 사용 X 설정.



​		==> Serial포트를 open하고 설정을 잡아놓은 상태

​		==> 전달되는 데이터(data frame)를 받을 수 있는 상태

6. 데이터를 주고 받을 수 있도록 SerialPort객체에서 Input/Output스트림을 얻는다.

   ==> 바이트 단위(io클래스관점)로 데이터가 송수신되므로 Reader,Writer계열의 스트림을 사용할 수 없고 InputStream, OutputStream객체를 사용해야 한다.

   SerialPort객체.getInputStream()

   SerialPort객체.getOutputStream()

   

7. 데이터 수신과 송신에 대한 처린

   - 쓰레드로 처리
   - 이벤트에 반응하도록 처리