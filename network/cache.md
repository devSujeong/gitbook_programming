# Cache

## 캐시가 없을 때

* 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
* 인터넷 네트워크는 느리므로 사용자 경험이 느려진다.

## 캐시 적용

* 캐시를 하면 캐시 가능 시간\(cache-control\)동안 네트워크를 사용하지 않아도 됨.
* 비싼 네트워크 사용량을 줄일 수 있어서 사용자 경험이 빨라진다.
* 캐시 유효시간이 초과하면 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다.

![](https://images.velog.io/images/dev_crystal/post/b5f289e1-7f20-4cbb-ab0b-b066a7d151ad/image.png)

## 검증 헤더 추가

캐시 유효시간이 만료되어 서버에 재요청을 할 때, 서버에는 2가지 상태가 존재한다. 1. 서버에서 데이터가 변경되지 않았음 2. 서버에서 데이터가 변경되었음.

1번의 경우라면 데이터를 또 받는 것보다 캐시를 재사용하는 게 효율적이다. 단, 클라이언트 데이터와 서버 데이터가 같다는 걸 알 수 있어야 한다.

> 실용적인 해결책으로, 데이터가 바뀌지 않았을 때 네트워크 다운로드가 발생해도 용량이 적은 헤더 정보만 다운로드 하도록 설계한다.

### Last-Modified

응답 헤더의 Last-Modified를 브라우저 캐시에 저장했다가 캐시 시간이 초과되어서 재요청할 때\(if-modified-since 조건부 요청 헤더\) 서버에서 데이터 최종 수정일을 비교하여 같으면 304 Not Modified를 보내고 http body를 보내지 않는다.

* 1초 미만 단위로 캐시 조정 불가능
* 날짜 기반의 로직 사용\(데이터 결과가 같지만 데이터를 건드려서 날짜만 다른 경우\)
* 서버에서 별도의 캐시 로직을 관리하고 싶은 경우

### ETag방식

캐시용 데이터에 임의의 고유한 버전 이름을 달아두고, 데이터가 변경되면 이 이름을 바꾸어서 변경함. 태그가 같으면 유지, 다르면 다시 받기!

응답헤더에서 ETag로 값을 보내면 브라우저 캐시에 이를 저장했다가 요청을 보낼 때 If-None-Match\(조건부 요청 헤더\)로 저장했던 값을 보내고 서버에서 태그가 같으면 304, 아니면 데이터 보냄.

* 캐시 제어 로직을 서버에서 완전히 관리
* ETag만 서버에 보내서 같으면 유지, 다르면 다시 받기

> 정리: 검증 헤더: ETag, Last-Modified 조건부 요청 헤더: If-Match, If-None-Match \(ETag값 사용\) / If-Modified-Since, If-Unmodifid-Since \(Last-Modified값 사용\)

## 캐시 제어 헤더

* Cache-Control: 캐시 제어
* Pragma: 캐시 제어\(하위 호환\)
* Expires: 캐시 유효 기간\(하위 호환\)

### Cache-Control

* Cache-Control: max-age

  캐시 유효시간, 초 단위

* Cache-Control: no-cache

  데이터는 캐시해도 되지만, 항상 origin 서버에 검증하고 사용

* Cache-Control: no-store

  데이터에 민감한 정보가 있으므로 저장하면 안됨.

### Pragma

* Pragma: no-cache

  http 1.0 하위 호환

### Expires

* Espires: Mon, 01 Jan 1990 00:00:00 GMT

  HTTP 1.0 하위 호환. 캐시 만료일을 정확한 날짜로 지정

## 프록시 캐시

예를 들어 origin server가 미국에 있고 클라이언트는 한국이라면 서버가 너무 멀어서 통신 시간이 지연됩니다. 이를 해소하고자 프록시 캐시 서버를 한국 어딘가 두어 사용하는 방법이 있습니다. 이때 클라이언트에게 있는 캐시는 private 캐시, 프록시 캐시 서버는 public 캐시라고 합니다.

* Cache-Control: public

  응답이 public 캐시에 저장되어도 됨.

* Cache-Control: private

  응답이 해당 사용자만을 위한 것이라 private 캐시에 저장함\(기본값\)

* Cache-Control: s-maxage

  프록시 캐시에만 적용되는 max-age

* Age: 60

  오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간\(초 단위\) 캐시 응답 때 나타나는데, max-age 시간 내에서 얼마나 흘렀는지 초 단위로 알려준다.  

## 캐시 무효화

> Cache-Control: no-cache, no-store, must-revalidate Pragma: no-cache

* no-cache: 데이터는 캐시해도 되지만, 항상 원서버에 검증하고 사용
* no-store: 데이터에 민감한 정보가 있으므로 저장하면 안됨.\(메모리에서 사용하고 최대한 빨리 삭제\)
* must-revalidate: 캐시 만료 후 최초 조회 시 원 서버에 검증해야 하며, 원 서버에 접근 실패 시 반드시 오류가 발생해야 함\(504\)
* Pragma는 HTTP1.0 하위 호환

