# ABROSS_FutureStudy
future study project

># MEMBER
>| PATH | METHOD | PARAMS |RETURN| DESC |
>|:----:|:------:|:-------|:-----|:----:|
>|'/join'|POST| string email<br>string passwd <br>string name<br>string aCode<br>string phone<br>string office<br>string schname<br>enum area<br>enum schtype<br>enum utype|Result:<br>1-정상<br>2-에러<br>3-중복<br><br>Msg:<br>"desc msg"| 인증을 다른 방식으로 하기때문에 activate를 입력하지 않음. <br><br> [예외상황]<br> 기존앱과 달리 가입 폼에서 인증이 구현 된다<br> 인증만 받고 가입하지 않은 경우, <br>해당 계정의 activate가 yet으로 세트. <br> yet이 아닌경우 해당 이메일로 다시 가입요청이 들어오면<br>다시 가입 절차를 진행할 수 있도록 한다.<br> 가입이 완료되지 않은 사용자가 기존앱에서 로그인함을 방지.<br><br>[수정사항]<br>기존앱과 인증 부분을 동일한테이블을 사용하지 않는다. <br> 몽고DB로 토큰을 따로 활용하거나<br> 새로운 테이블을 만드는 방향으로 진행해야할 것 같다.<br> 완벽하게 가입이 이루어진 후에 해당 테이블에 기록한다. |
>|'/update'|POST|string email<br>string passwd<br>string phone<br>string office<br>string schname<br>enum area<br>enum shctype<br>enum utype|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|회원의 비밀번호,모바일,사무실전화,학교명,<br>지역,학교타입,유저타입을 변경가능|
>|'/delete'|POST|string email<br>string passwd|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|email과 passwd 검증 후 회원탈퇴처리|
>|'/login' |POST |string email <br> string passwd |Result:<br>1-정상<br>2-에러<br>3-아이디없음<br>4-비번틀림<br><br>Msg:<br>"desc msg" <br><br> Data:<br>{userDataObject}| 로그인시 아이디와 패스워드 확인 후,<br> 일치하면 last_login을 현재 시각으로 변경 |
>|'/reqAuth'|POST|string email|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|해당 이메일 인증요청 처리<br>몽고DB 인증 토큰 생성<br>해당 토큰 요청이메일로 발송|
>|'/chkAuth'|POST|string email <br> string aCode <br>|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|토큰 테이블 참조 후, 이메일 인증결과 리턴|
