# Internet

## Hyper Text Transfer Protocol

웹 브라우저와 웹 서버가 통신을 하기 위해 사용하는 통신 규약.

서버와 클라이언트 사이에서 어떻게 메시지를 교환할 지 정해 놓은 규칙

**클라이언트의 요청**(Request)과 **서버의 응답**(Response)로 구성되어 있다.

### 요청 (Request)

### **Request Header Format**

GET /doc/test.html HTTP/1.1                             // Request Line

Host : localhost:8080                                          //  요청하는 웹 사이트의 주소, 

Accept: image/gif, image/jpeg, */*                    //

Accept-Language: en-us                                   //    

Accept-Encoding: gzip, deflate, br                    // 데이터 전송 어떤 압축방식을 지원하는지 설명

User-Agent: Mozilla/4.0                                     // 유저 컴퓨터, 웹브라우저 정보

Content-Length: 35                                            // 

                                                                            // *Header와 Body를 구분 짓는 Blank Line*

bookId=12345&author=Tan+Ah+Teck               // Request Message Body

### 응답 (Response)

### **Response Header Format**

HTTP/1.1 200 OK                             // Response Line

…

Content-Type : text/html

### **Status Code**

1xx : 정보를 주기 위한 응답
2xx : 통신 성공
3xx : Redirection (다른 웹 페이지로 응답 시키는 것)
4xx : 클라이언트 에러
5xx : 서버 에러

통신이 성공되면 서버는 요청한 데이터(HTML 등)을 응답하게 된다.

## HTTP와 HTTPS의 차이

HTTPS의 S는 SECURE의 약자

초기엔 웹을 통해 민감한 정보를 다루지 않았으나 시간이 지나 

군사, 금융, 사생활 등을 웹에서 다루게 되면서 보안의 필요성을 느낌.

HTTP에 암호화를 더하여 나온 것이 HTTPS이다.

HTTPS를 사용하면 전송하고 있는 내용이 가로채여도 

그 안에 무슨 내용이 담겨 있는지 당사자 외에는 알 수 없다.

### Cache

저장한다는 뜻

한 번 웹사이트에 접속해서 어떠한 내용을 다운로드 받았다면 그 다음에 접속할 때 또 다운로드 받을 필요 없이 이미 다운로드 받은 저장된 파일을 읽어서 성능을 향상시키는 기법

단점 : 캐시의 내용이 갱신 되어도 웹 브라우저는 그 사실을 알 수 없음.

*(웹 브라우저에서 단축키로 강제 캐시 갱신이 가능)*

- *Windows : Ctrl + F5*
- *MacOS : Cmd + R*
- *Linux : F5*

캐시 갱신은 일반 유저가 알 수 없어 

Cache-Control, pragma 등 헤더 값들로 캐시를 제어하는 기술 사용

### Cookie

서버를 통해 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일

쇼핑몰에서 장바구니 이용이나 사이트의 로그인 유지 기능에 사용된다.

웹 사이트를 방문할 때 이전에 처리했던 장바구니 담기, 로그인과 같은 기록들을 

웹 사이트와 웹 브라우저가 쿠키 파일을 통해 기억하는 것.

쿠키를 이용해서 쿠키 값을 웹 브라우저에 설정하면 접속할 때마다 그 설정됐던 쿠키 값을 서버에 전송하는 것을 통해서 사용자의 상태를 유지할 수 있고 사용자를 식별할 수 있음

쿠키보다도 훨씬 더 많은 정보를 저장하면서도

보안적으로 더 우수한 Web Storage라는 기술도 있다.

### Proxy

클라이언트와 서버 사이에서 데이터를 전달해 주는 서버

중간에 있는 서버가 캐시를 대신해 주거나, 보안과 관련된 공격을 막아 주거나, 

적당히 사용자 요청을 여러 대의 서비스로 분산해주는 것과 같은 역할을 

프록시 서버들이 대신 해줄 수 있다. (그 외에도 많은 기능 가능)
