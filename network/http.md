# HTTP

## History

* HTTP/0.9\(1991년\): GET 메서드만 지원, HTTP 헤더 없음
* HTTP/1.0\(1996년\): 메서드, 헤더 추가
* HTTP/1.1\(1997년\): 가장 많이 사용. RFC2068\(1997\) -&gt; RFC2616\(1999\) -&gt; RFC7230~7235\(2014\)
* HTTP/2.0\(2015년\): 성능 개선
* HTTP/3.0\(진행중\): TCP 대신에 UDP 사용. 성능 개선

{% hint style="info" %}
현재는 1.1을 주로 사용하지만 2, 3도 점점 증가 추세에 있습니다.
{% endhint %}

## Features

### 클라이언트-서버 구조

Request, Response 구조입니다. 클라이언트가 서버에 요청을 보내고 응답을 대기하고 서버는 요청에 대한 결과를 만들어서 응답합니다.

### 무상태 프로토콜\(stateless\), 비연결성

서버가 클라이언트의 상태를 가지고 있지 않습니다. 서버의 확장성이 높아지지만 클라이언트가 추가 데이터를 전송해야 하는 단점이 있습니다. 또한 로그인처럼 무상태로는 진행할 수 없는 경우도 있습니다.

HTTP는 비연결성이므로 클라이언트는 아무 서버와 연결해도 되므로 기존 연결된 서버에 문제가 생겨도 클라이언트는 문제가 되지 않습니다. 한 서버에 클라이언트가 몰리지 않도록 분산도 가능하구요. 따라서 서버 자원을 매우 효율적으로 사용할 수 있습니다. 단점은 TCP/IP 연결을 새로 맺어야 하므로 연결 시간이 증가합니다. 이 문제는 지속 연결\(Persistent Connections\)로 문제를 해결했습니다.

### HTTP 메시지

**HTTP MESSAGE STRUCTURE  
-** 시작라인: Request-line\(메서드, 요청 타깃, http 버전\) or status-line\(http 버전, status-code, reason-phrase\)  
- 헤더: http전송에 필요한 모든 부가 정보. 표준 헤더가 이미 너무 많지만 필요시 임의의 헤더 추가도 가능함. 형태-&gt; field-name: field-value  
- 공백라인:   
- 메시지 바디: 실제로 전송할 데이터.

### 단순함, 확장 가능

## HTTP Method

이상적으로는 API의 URI는 리소스를 식별하는 데 주안점을 줍니다. 따라서 명사들로 이루어지며, 동사에 해당하는 액션은 Method로 처리합니다.

* GET: 리소스 조회
* POST: 요청 데이터 처리. 주로 등록에 사용
* PUT: 리소스를 대채, 해당 리소스가 없으면 생성
* PATCH: 리소스 부분 변경
* DELETE: 리소스 삭
*  HEAD: 리소스를 조회하는데 메시지 부분을 제외하고 상태와 헤더만 반환
* OPTIONS: 대상 리소스에 대한 통신 가능 옵션을 설명
* CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
* TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수

### GET

리소스 조회  
서버에 전달하고 싶은 데이터는 쿼리 파라미터\(, 쿼리 스트링\)을 통해서 전달  
메시지 바디를 사용해서 데이터를 전달할 수 있지만 지원하지 않는 곳이 많아서 비권장.

### POST

요청 데이터 처리  
메시지 바디를 통해 서버로 요청 데이터 전달  
서버는 요청 데이터를 처리하는데 주로 전달된 데이터로 신규 리소스를 등록하거나 프로세스 처리에 사용

### PUT

기존 리소스가 있으면 대체하고, 없으면 생성합니다. 리소스의 일부만 대체하는 게 아니라 전부 대체하는 것이고 클라이언트가 리소스를 식별\(리소스 위치를 알고 URI 지정\)한다는 점이 post와의 차이점 입니다.

클라이언트가 리소스 URI를 알고 있어야한다. 클라이언트가 직접 리소스의 URI를 지정하나.

### PATCH

리소스를 부분 변경합니다. 대체 아님.

### DELETE

리소스 제거.

### 메서드의 속성

* 안전\(safe\): 호출해도 리소스를 변경하지 않는다.
* 멱등\(idempotent\): 몇 번 호출하든 결과가 똑같다. \(post는 멱등하지 않다.\)
* 캐시가능\(cacheable\): 응답 결과를 캐시해서 사용해도 되는지 여부인데 실제로는 구현이 쉬운 GET, HEAD정도만 캐시로 사용함.

## Method 활용

### 데이터 전달 방식

1\) 쿼리 파라미터를 통한 데이터 전송: GET, 주로 검색  
2\) 메시지 바디를 통한 데이터 전송: POST, PUT, PATCH.

### 데이터 전송 상황

* 정적 데이터 조회
* 동적 데이터 조회
* html form을 통한 데이터 전송
* http api를 통한 데이터 전

### URI 설계 개

* Control URI: 메소드만으로 동사를 표현하기엔 제약이 있으므로 동사로 된 리소스 경로 사용.
* 문서\(document\): 파일 하나, 객체 인스턴스, 데이터베이스 row
* collection: 서버가 관리하는 리소스 디렉터리.
* store: 클라이언트가 관리하는 자원 저장소. 클라이언트가 리소스의 uri를 알고 관

## HTTP status code

### 1xx Informational

요청이 수신되어 처리 중. 거의 사용하지 않음

### 2xx Successful

요청 정상 처리

* **200** Ok
* **201** Created. 요청 성공해서 새로운 리소스가 생성됨. 응답 헤더의 Location헤더 필드로 리소스를 확인할 수 있음
* **202** Accepted. 요청이 접수되었으나 처리가 완료되지 않았음. ex\) 배치
* **204** No Content. 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음

### 3xx Redirection

요청을 완료하려면 추가 행동이 필요. 300대 응답 결과에 Location 헤더가 있다면, 웹브라우저는 Location 위치로 자동 이동한다.

* **300** Multiple Choices: 사용 안함.
* **301** Moved Permanently. 영구 리다이렉션으로 원래 url을 사용 안하게 됨. 리다이렉트 요청 메서드가 GET으로 변하고 본문이 제거될 수 있음.
* **302** Found: 일시적인 리다이렉션으로 원래 url도 사용하고, 임시적으로 다른 곳으로 보냄. 예를 들면 주문 후 주문 완료 페이지로 보내는 것. 리다이렉트 시 요청 메서드가 get으로 변하고 본문이 제거될 수 있음
* **303** See other: 302와 같은 일시적인 리다이렉션으로 리다이렉트시 요청 메서드가 get으로 변경. 302와 똑같으나 원래 302의 의도는 get으로 변하는 게 아니었는데 브라우저가 그렇게 구현을 해버려서 명확하게 스펙으로 새로 만듦.
* **304** Not Modified: 캐시를 목적으로 사용하며 클라이언트에게 리소스가 수정되지 않았음을 알려줍니다. 따라서 클라이언트는 로컬 pc에 저장된 캐시를 재사용합니다. 로컬 캐시를 사용해야 하므로 응답에 바디 메시지가 없어야 합니다.
* **307** Temporary Redirect: 302와 같은 일시적인 리다이렉션이나 리다이렉트 시 요청 메서드와 본문을 유지한다.
* **308** Permanent Redirect: 영구 리다이렉션으로 301과 기능이 같으나 리다이렉트시 요청 메서드와 본문을 유지한다.

### 4xx Client Error

클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음. 클라이언트가 잘못된 요청을 보내는 것이므로 같은 조건에서는 항상 실패해야 함.

* **401** Unauthorized: 클라이언트가 해당 리소스에 대한 인증이 필요함. 401 오류 발생 시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명.
* **403** Forbidden: 서버가 요청을 이해했지만 승인을 거부함. 인증 자격은 있지만 접근 권한이 불충분한 경우.
* **404** Not Found: 요청 리소스가 서버에 없거나 클라이언트가 권한이 부족한 리소스에 접근하면 해당 리소스를 아예 숨기고 싶을 때 사용.

### 5xx Server Error

서버 오류, 서버가 정상 요청을 처리하지 못함. 서버의 문제이기 때문에 재시도하면 성공할 수도 있음.

* **500** Internal Server Error: 애매하면 500 에러. 서버 내부 문제.
* **503** Service Unavailable: 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음. Retry-After 헤더 필드로 얼마 뒤에 복구되는지 보낼 수도 있음

## HTTP Header <a id="http-header&#xB780;"></a>

http 전송에 필요한 모든 부가정보를 다룸. 필요시 임의의 헤더도 추가 가능하다. 표준 헤더가 너무 많다.

## 헤더 분류 <a id="&#xD5E4;&#xB354;-&#xBD84;&#xB958;"></a>

* Gneral 헤더: 메시지 전체에 적용되는 정보
* Request 헤더: 요청정보
* Response 헤더: 응답 정보
* Entity 헤더: 엔티티 바디 정보를 해석할 수 있는 정보 제공.

## RFC723x 변화 <a id="rfc723x-&#xBCC0;&#xD654;"></a>

오랫동안 표준으로 사용되던 1999년의 RFC2616이 폐기되고 2014년 새로운 RFC7230~7235 표준이 등장함.

* 엔티티 -&gt; 표현\(Representation\)으로 명칭이 바뀜.

### Representation header <a id="representation-header"></a>

* Content-Type: 표현 데이터의 형식 미디어 타입, 문자 인코딩 등으로 표현. ex\) Content-Type: text/html; charset=utf-8 or application/json or image/png
* Content-Encoding: 표현 데이터의 압축 방식. 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가. 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제. ex\) gzip, deflate, identity
* Content-Language: 표현 데이터의 자연 언어. ex\)ko, en, en-US
* Content-Length: 표현 데이터의 길이. 바이트 단위. Transfer-Encoding을 사용하면 이 헤더는 사용하면 안됨.

#### Content-negotiation\(협상\): 클라이언트가 선호하는 표현 요청. <a id="content-negotiation&#xD611;&#xC0C1;-&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xAC00;-&#xC120;&#xD638;&#xD558;&#xB294;-&#xD45C;&#xD604;-&#xC694;&#xCCAD;"></a>

* Accept: 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset: 클라이언트가 선호하는 문자 인코딩
* Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
* Accept-Language: 클라이언트가 선호하는 자연 언어

여러가지를 원할 수 있으므로 우선순위를 넣어서 보낼 수도 있다.  
1\) Quality Values\(q\)로 구현 가능하고, 값은 0~1사이인데 클수록 우선순위가 높다. 생략하면 1이다.  
2\) q가 없다면 구체적일수록 우선순위가 높다.

ex1\) Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7  
1. ko-KR  
2. ko  
3. en-US  
4. en

ex2\) Accept: text/_, text/plain, text/plain;format=flowed,_ /  
_1. text/plain;format=flowed  
2. text/plain  
3. text/_  
4. _/_

### 전송방식 <a id="&#xC804;&#xC1A1;&#xBC29;&#xC2DD;"></a>

* Content-Length: 단순 전송. ex\) Content-Length: 3423
* Content-Encoding: 압축 전송 ex\) Content-Length: gzip
* Transfer-Encoding: 분할 전송 ex\) Transfer-Encoding: chunked
* Range, Content-Range: 범위 전송 ex\) Range: bytes=1001-2000 or Content-Range: bytes 1001-2000 / 2000

### 일반정보 <a id="&#xC77C;&#xBC18;&#xC815;&#xBCF4;"></a>

* From: 유저 에이전트의 이메일 정보. 검색 엔진에서 주로 사용.
* Referer: 이전 웹페이지 주소. 이걸 이용해서 유입 경로 분석 가능
* User-Agent: 유저 에이전트 애플리케이션 정보. 통계 정보.
* Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보. 응답에서 사용.
* Date: 메시지가 생성된 날짜. 응답에서 사용.

### 특별한 정보 <a id="&#xD2B9;&#xBCC4;&#xD55C;-&#xC815;&#xBCF4;"></a>

* Host: 요청한 호스트 정보\(도메인\). 필수. 서버에 여러 호스트가 있을 수 있기 때문.
* Location: 페이지 리다이렉션. 3xx 응답 결과에서 자동 이동을 위해서. 201과 쓰이면 리소스 url이 됨.
* Allow: 허용 가능한 HTTP method. 405 응답에 포함되어야 함.
* Retry-After: 유저가 다음 요청을 하기까지 기다려야 하는 시간. 503과 쓰여 서비스가 언제까지 불능인지 알려줄 수 있음. 날짜 표기 혹은 초단위로 표기함.

### 인증 <a id="&#xC778;&#xC99D;"></a>

* Authorization: 클라이언트 인증 정보
* WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의. 401과 함께 사용.

### 쿠키 <a id="&#xCFE0;&#xD0A4;"></a>

* Set-Cookie: 서버에서 클라이언트로 쿠키 전달\(응답\)
* Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청 시 서버로 전달



