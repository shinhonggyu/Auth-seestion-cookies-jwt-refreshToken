**인증과 인가**

1. 인증하기(로그인) Request Header
2. 인증 유지하기 Browser Storage -> 보안 문제
3. 안전하게 인증하기 Server -> 서버 과부하
4. 효율적으로 인증하기 Token
5. 다른 채널을 통해 인증하기 OAuth

로그인을 계속 하지않으려면 storage에 정보 저장은 해야한다..

Local Storage vs Session Storage vs Cookies  
쿠키와 다르게 웹 스토리지 객체는 네트워크 요청 시 서버로 전송되지 않는다.

**session 과 cookies**

1. 회원가입후 로그인(인증)할때 정보가 맞으면 서버는 너의 session id을 만들고 서버메모리에 저장한다.

2. 너의 session id를 cookie에 실어 보낸다.
   Set-Cookie: JESSIONID=123

3. Auth request를 할때 cookie: JESSIONID=123

4. 서버는 sesstion-id를 서버메모리에 저장된 session data과 비교한다.

5. 응답을 보낸다.

- **단점**
- 한번 인증해서 sesstion을 받았을때 담음요청은 sesstion으로만 요청 하게된다.
- 서버를 여러개 두었을 경우 서버 하나당 자체에서 세션을 관리하고 있을경우 문제가 생긴다.
- 세션 저장소라는곳을 만들어 한곳에서 관리 할수있다.
- client가 많아질경우 서버과부하

**JWT**

1. 회원가입후 로그인(인증)할때 정보가 맞으면 서버는 secret Key를 이용해 JWT을 만든고  
   저장하지않고 만든 JWTokent을 보낸다.  
   JWToken 에는 유저 id, admin 여부, 만료시간등 이 담겨있다.

2. Auth request 할때 request header에 JWToken을 담아 보낸다.

3. 서버는 JWToken을 secret Key로 토큰 유효성 검사를 한다.

4. 유효하다면 사용자 정보를 파악한다. 이름(어떤사용자인지), 권한, 만료시기

5. 응답을 보낸다.

토큰을 공유해서 모든 독립적인 서버에서 사용할수있다

**Refresh Token**

1. 로그인(인증)할때 정보가 맞으면 서버는 secret Key를 이용해
   access token과 refresh token을 만든다.
2. access token은 저장하지않고 refresh token만 따로 저장소에 저장한다.

3. access token과 refresh token 둘다 응답의 헤더로 보내서 클라이언트가 둘다 저장하게된다.

4. access token을 활용해서 client가 요청을 보낸다.

5. access token 을 보냈는데 만료됬을 경우 서버는 만료됬음을 알리고 client 에서 자동으로 access token과 refresh token을 함께 다시 서버로 보내게 되고 서버는 refresh token을 참고해서 저장소와 비교하고 새로 갱신한 access token을 보내게 된다.

- **핵심**
- 토큰으로 상태관리를 하기에 따로 세션을 둘 필요가 없다.
- 토큰 관리를 해야한다. 결국 토큰도 탈취당할 수 있다.

출처: 우아한Tech 토니님 강의
