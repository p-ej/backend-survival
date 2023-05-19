# HTTP Client

### TCP/IP 통신
- TCP/IP를 기반으로 HTTP 가 올라감
  - HTTP/3만 UDP를 이용한 QUIC 프로토콜을 사용
  - HTTP/2는 SPDY 프로토콜을 기반으로 한다. 
- 1.1 기본적인것
- 인터넷 프로토콜 스위트
- 전송계층
  - TCP(전송 제어 프로토콜) : 연결이 필요, 전달 및 순서보장 (전화)
  - UDP: 연결하지 않고 데이터를 보냄, 전달 및 순서를 보장하지 않음. (편지)


### 소켓
소켓
- 소켓과 소켓 API를 구분해야 헷갈리지 않는다. 
- 버클리 소켓: 소켓프로그래밍을 위한 API , 소켓 API
- 네트워크 소켓: 프로세스 간 통신의 종착점(엔드포인트)
- 기본적으로 파일과 유사하게 다룰 수 있다. (열고 읽고쓰고 닫고)
  - 유닉스에서는 파일 디스크립터의 일종
- 자바에서는 키보드 입력, 화면출력, 파일 입출력등과 마찬가지로 Stream(자바 8에 도입된 Stream API가 아님)으로 다룰 수 있따.


### TCP 통신 순서
TCP 통신 순서
? 서버가 닫혀있으면 어떡하지 ?
1. 서버는 접속 요청을 받기 위한 소켓을 연다. LIsten
2. 클라이언트는 소켓을 만들고, 서버에 접속을 요청함 Connect
3. 서버는 접속 요청을 받아 클라이어늩와 통신할 소켓을 따로 만듦 Accept
4. 소켓을 통해 서로 데이터를 주고 받는다. Send & Receive 반복
5. 통신을 마치면 소켓을 닫음 Close -> 상대방은 Receive로 인지할 수 있음. 



### HTTP 클라이언트 
- 간단한 구현 실습진행
- Gradle init 을 이용한 프로젝트 세팅

1. 소켓 연결
2. 요청
3. 응답
4. 닫기



## 학습 키워드
* TCP/IP 통신
* TCP와 UDP
* Socket과 Socket API 구분
* URI와 URL
* 호스트(host)
  * IP 주소
  * Domain name
  * DNS
* 포트(port)
* path(경로)
* Java text blocks
* Java InputStream과 OutputStream
* Java try-with-resources


[![Hits](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction.svg)](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction/)