
## HTTPS 인증 과정
HTTPS 인증 과정에 대해 다시 정리해보자
HTTPS 인증 과정의 목적은 **서버 - 클라이언트 사이에 주고받을 데이터의 암호화 알고리즘을 결정, 동일한 대칭키를 공유**하는 것이다.

대칭키를 pure하게 네트워크를 사용하여 공유하면 탈취의 위험이 굉장히 크다. 그래서 이 대칭키를 암호화하여 서버와 클라이언트만 알 수 있게끔 공유한다.

## 절차

> -- TCP Handshake 시작 -- 
> 1. 3-way handshake
> 
> -- SSL Handshake 시작 -- 
> 
> (Client  -> Server) 2. Client Hello
> (Server ->  Client) 3. Server Hello, Certificate,  Server Key Exchange,  Server Hello Done
> (Client  -> Server) 4. Client Key Exchange, Change Ciper Spec
> (Server ->  Client) 5. Server / Client SSL Handshake Finished
> 
> -- SSL Handshake 종료 --

### 1. 3-way handshake
HTTPS 가 TCP 기반의 프로토콜이기 때문에 SSL Handshake 에 앞서 연결을 생성하기 위한 과정이다.

### 2. `Client Hello`
클라이언트에서 통신하고 싶은 서버로 연결을 시도하는 패킷을 전송한다.
- 패킷 내용 : 클라이언트가 사용 가능한 cipher suite 목록, session id, ssl protocol version ...

### 3. `Server Hello`
`Client Hello` 에 대해 서버가 응답 패킷을 보낸다.
- 패킷 내용 : 클라이언트에서 받은 cipher suite 목록 중 선택한 1개의 cipher suite, ssl protocol version ...

### 3. `Certificate`,  `Server Key Exchange`,  `Server Hello Done`
`Certificate` : Server 의 SSL 인증서 내용을 전달
 - 패킷 내용 : 인증서의 signature algorithm, public key info

`Server Key Exchange` : Certificate 내의 SSL 인증서에 서버의 공개키(public key)가 없는 경우, 서버가 직접 서버의 공개키를 전달
- 인증서 내부에 서버의 공개키가 존재한다면 Server Key Exchange 는 생략된다

`Server Hello Done` : 서버가 행동을 마쳤다는 의미이다.

### 4. `Client Key Exchange`, `Change Ciper Spec`
`Client Key Exchange` : `Certificate` 나 `Server Key Exchange`에서 확보한 서버의 공개키로 통신할 데이터를 암호화할 대칭키를 암호화한다. (서버에게 외부 탈취없이 전달하기 위해!)

`Change Ciper Spec` : SSL Handshake 의 완료를 알림. 

### 5. Server / Client SSL Handshake Finished
서버는 클라이언트가 전달한 암호화된 대칭키(비밀키)를 받았다. 이 키는 서버의 공개키로 암호화되었으니, 서버의 비밀키로 복호화해서 얻을 수 있다. 이로서 클라이언트와 서버가 외부의 탈취없이 동일한 대칭키를 공유할 수 있게 됐다.

## 참고자료
[HTTPS 통신 원리 쉽게 이해하기 (Feat. SSL Handshake, SSL 인증서)](https://nuritech.tistory.com/25)
