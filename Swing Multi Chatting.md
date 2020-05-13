Swing

java fx(css,javascript)



이클립스 플러그인 window builder툴을 설치하고 드래그앤 드랍으로 화면 디자인



1. 서버 실행
   - ChatServerView (ChatServerListener)
     - 서버가 여러가지 상태를 확인할 수 있는 창
2. 클라이언트 접속
   - ChatLogin (ChatLoginListener)을 먼저 실행해서 로그인(ip, port, 채팅 nickname)
   - ClientChatView가 실행 (ClientChatListener)
     - 클라이언트가 채팅하는 화면



```java
//===1. 소켓 프로그래밍을 하기 위한 객체를 준비
	 ServerSocket server;
	 Socket socket;
//=========2. 클라이언트의 접속을 기다리면서 서비스를 시작할 수 있는 메소드를 정의========
	public void serverStart(int port) {
		try {
			server = new ServerSocket(port);
			taclientlist.append("사용자 접속 대기 중\n");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
//==========3. 사용자 접속을 위한 메소드 정의================
	public void connection() {
		try {
			socket = server.accept();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
```



