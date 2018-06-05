# ABROSS_FutureStudy
>future study project

# Requirements
>## Video
> 관리자가 영상을 등록/수정/삭제 할 수 있으며<br>
> 권한이 있는 사용자가 조회가능하게 서비스 한다.

>## Member
> 기존에 개발중인 앱과 동일한 멤버 정보를 공유할 수 있도록 <br>
> 기존 회원 정보 데이터베이스만 공유 한다. <br>
> 기존 앱의 데이터베이스 정보에 대한 생성/수정/삭제 사이클이 문제가 발생하지 않도록 작업한다.<br>
> *( 현재 만들어진 테이블 그대로를 회원 정보 테이블로 사용하고 그 외의 정보는 회원의 id를 통해<br>
>    참조할 수 있는 콜렉션으로 만들어 작업한다. )*

>## Notice
> 개발할 웹 서비스는 기존 앱서비스와 다른 서비스이기 때문에 공지사항은 공유하지 않는다.

>## Payment
> 회원들이 특정 등급에 대한 신청 및 결제에 대한 내역을 조회/승인 할 수 있도록 관리자 페이지 구성
> 결제정보관련기록 Collection : vip , 현재 유효한 회원 : currentVip


# REST API
># MEMBER
>| PATH | METHOD | PARAMS |RETURN| DESC |
>|:----:|:------:|:-------|:-----|:----:|
>|'/join'|POST| string email<br>string passwd <br>string name<br>string aCode<br>string phone<br>string office<br>string schname<br>enum area<br>enum schtype<br>enum utype|Result:<br>1-정상<br>2-에러<br>3-중복<br><br>Msg:<br>"desc msg"| 인증을 다른 방식으로 하기때문에 activate를 입력하지 않음. <br><br> [예외상황]<br> 기존앱과 달리 가입 폼에서 인증이 구현 된다<br> 인증만 받고 가입하지 않은 경우, <br>해당 계정의 activate가 yet으로 세트. <br> yet이 아닌경우 해당 이메일로 다시 가입요청이 들어오면<br>다시 가입 절차를 진행할 수 있도록 한다.<br> 가입이 완료되지 않은 사용자가 기존앱에서 로그인함을 방지.<br><br>[수정사항]<br>기존앱과 인증 부분을 동일한테이블을 사용하지 않는다. <br> 몽고DB로 토큰을 따로 활용하거나<br> 새로운 테이블을 만드는 방향으로 진행해야할 것 같다.<br> 완벽하게 가입이 이루어진 후에 해당 테이블에 기록한다. |
>|'/update'|POST|string email<br>string passwd<br>string phone<br>string office<br>string schname<br>enum area<br>enum shctype<br>enum utype|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|회원의 비밀번호,모바일,사무실전화,학교명,<br>지역,학교타입,유저타입을 변경가능|
>|'/delete'|POST|string email<br>string passwd|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|email과 passwd 검증 후 회원탈퇴처리|
>|'/login' |POST |string email <br> string passwd |Result:<br>1-정상<br>2-에러<br>3-아이디없음<br>4-비번틀림<br><br>Msg:<br>"desc msg" <br><br> Data:<br>{userDataObject}| 로그인시 아이디와 패스워드 확인 후,<br> 일치하면 last_login을 현재 시각으로 변경 |
>|'/reqAuth'|POST|string email|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|해당 이메일 인증요청 처리<br>몽고DB 인증 토큰 생성<br>해당 토큰 요청이메일로 발송|
>|'/chkAuth'|POST|string email <br> string aCode <br>|Result:<br>1-정상<br>2-에러<br><br>Msg:<br>"desc msg"|토큰 테이블 참조 후, 이메일 인증결과 리턴|

<br>
># VIP
>| PATH | METHOD | PARAMS | RETURN | DESC |
>|:----:|:------:|:------:|:------:|-----:|
>|'/getAll'|POST|string utype|Result:<br>1-정상<br>2-에러<br>3-권한없음<br><br>MSG:<br>관련메세지<br><br>Data:<br>vip[{stat:'wait'/'ok',}]|모든 결제요청승인정보 조회|
>|'/get'|POST|string utype<br>string id|Result:<br>1-정상<br>2-에러<br>3-권한없음<br><br>MSG:<br>관련메세지<br><br>Data:<br>vip|특정 회원의 모든 결제요청승인정보 조회|
>|'/update'|POST|||결제가 확인된 회원에 한해 등급변경|
>|'/request'|POST|||등급변경에 대한 요청|
