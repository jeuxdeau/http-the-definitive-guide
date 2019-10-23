# 03. HTTP 메시지

## 3.1 메시지의 흐름

**HTTP 메시지**

* 정의: HTTP 애플리케이션 사이에서 주고받은 데이터 블록들
* 구성: 1\) 텍스트 메타 정보 + 2\) 데이터\(optional\)
  * 텍스트 메타 정보: 메시지의 내용과 의미를 설명함
* 방향: 인바운드, 아웃바운드, 업스트림, 다운스트림

### 3.1.1 인바운드: 서버 방향 송신

* 인바운드: 사용자 에이전트 → 서버
* 아웃바운드: 서버 → 사용자 에이전트

### 3.1.2 다운스트림: 메시지의 방향

* 모든 메시지는 다운스트림으로 흐른다
* 요청/응답 메시지 모두 다운스트림으로 흐른다
* \(업스트림\) 발송자 → \(다운스트림\) 수신자

## 3.2 메시지의 각 부분

* 시작줄 + 헤더 + 본문
* 시작줄과 헤더는 CRLF로 구분되나, 그 자리에 개행 문자가 오기도 하고, 아무 것도 안 오기도 한다. 견고한 애플리케이션이라면 이런 모든 상황을 대처할 수 있어야 한다.

### 3.2.1 메시지 문법

* 요청 메시지: 서버에 동작 요구
* 응답 메시지: 요청의 결과를 클라이언트에게 돌려줌

  ```text
  # 요청 메시지
  GET /specials/saw-blade.gif HTTP/1.0
  Host: www.joes-hardware.com

  <메서드> <요청 URL> <버전>
  <헤더>

  <엔티티 본문>

  # 응답 메시지
  HTTP/1.0 200 OK
  Content-Type: image/gif
  Content-length: 8572

  <버전> <상태 코드> <사유 구절>
  <헤더>

  <엔티티 본문>
  ```

**요청 URL**

* 요청 대상 리소스의 완전한 URL 또는 URL의 경로 구성 요소
* 불완전한 URL이더라도 서버는 생략된 호스트/포트가 자신을 가리키는 것으로 간주함

**사유 구절**

* 상태 코드의 사람이 이해할 수 있는 버전
* 특별한 규칙 없음
* `HTTP/1.0 200 NOT OK` 와 `HTTP/1.0 200 OK`는 동등하게 성공을 의미하는 것으로 처리

**엔티티 본문**

* 임의의 데이터 블록
* 엔티티 블록을 포함하지 않는 메시지들은 그냥 CRLF로 끝남

### 3.2.2 시작줄

**메서드**

> [3.3 메서드](./#3-3)

**상태 코드**

> [3.4 상태 코드](./#3-4)

**버전 번호**

* 요청, 응답 메시지에 모두 포함
* 응답을 보낸 애플리케이션이 이해하는 가장 높은 HTTP 버전을 가리킴
* HTTP 대화 상대의 능력과 메시지 형식에 대한 단서
* HTTP 버전을 소수라고 생각하면 안 됨 \(ex. HTTP/2.22는 HTTP/2.3보다 크다\)

### 3.2.3 헤더

**헤더 분류**

* 일반 헤더: 요청, 응답 양쪽에 모두 나타날 수 있음
* 요청 헤더: 요청에 대한 부가 정보 제공
* 응답 헤더: 응답에 대한 부가 정보 제공
* Entity 헤더: 본문 크기와 콘텐츠 혹은 리소스 그 자체를 서술
* 확장 헤더: 명세에 정의되지 않은 새로운 헤더

**헤더를 여러 줄로 나누기**

* 긴 헤더 줄을 여러 줄로 쪼개서 더 읽기 좋게 만들 수 있다.

  ```text
  # 두 헤더 줄은 같음
  Server: Test Server
      Version 1.0
  Server: Test Server Version 1.0
  ```

### 3.2.4 엔티티 본문

이미지, 비디오, HTML 문서, 소프트웨어 애플리케이션, 신용카드 트랜잭션, 전자우편 등 여러 종류의 디지털 데이터

### 3.2.5 버전 0.9 메시지

* 요청: 메서드 + 요청 URL \(ex. `GET /specials/saw-blade.gif`\)
* 응답: 엔티티
* 버전 정보, 상태 코드, 사유 구절, 헤더 등이 없음

## 3.3 메서드

* 클라이언트 측에서 서버가 리소스에 대해 수행해주길 바라는 동작을 요청 메시지에 전달
* 한 단어로 되어 있음 \(ex. `GET`, `HEAD`, `POST`\)
* 확장 메서드: 특정 서버가 추가로 구현한 HTTP 메서드

  | 메서드 | 설명 | 본문 |
  | :--- | :--- | :--- |
  | GET | 서버에서 어떤 문서를 가져온다. | X |
  | HEAD | 서버에서 어떤 문서에 대해 헤더만 가져온다. | X |
  | POST | 서버가 처리해야 할 데이터를 보낸다. | O |
  | PUT | 서버에 요청 메시지의 본문을 저장한다. | O |
  | TRACE | 메시지가 프락시를 거쳐 서버에 도달하는 과정을 추적한다. | X |
  | OPTIONS | 서버가 어떤 메서드를 수행할 수 있는지 확인한다. | X |
  | DELETE | 서버에서 문서를 제거한다. | X |

* 모든 서버가 모든 메서드를 구현하지는 않음
* 메서드는 대부분 제한적으로 사용됨 \(아무나 메서드를 사용할 수 없음\)

### 3.3.1 안전한 메서드

* HTTP 요청의 결과로 서버에 어떤 작용도 없는 것
* 안전한 메서드가 서버에 작용을 유발하지 않는다는 보장은 없음
* 안전한 메서드의 목적은 안전하지 않은 메서드가 사용될 때 사용자들에게 그 사실을 알려주는 것임

### 3.3.2 GET

* 서버에게 리소스를 달라고 요청
* HTTP/1.1은 서버가 GET을 구현할 것을 요구함

### 3.3.3 HEAD

* GET처럼 행동하지만, 응답으로 헤더만 돌려주는 것
* GET으로 얻는 헤더와 같은 헤더이고 엔티티 본문만 없는 것
* 리소스가 삭제되거나 변경되었는지 검사
* 리소스를 가져오지 않고 타입 등 정보를 알아냄
* HTTP/1.1 준수를 위해서는 HEAD가 반드시 구현되어 있어야 함

### 3.3.4 PUT

* 서버에 요청의 본문을 가지고 요청 URL의 이름대로 새 문서를 만드는 것
* 이미 해당 URL이 존재한다면 요청의 본문으로 교체하는 것
* 많은 웹 서버가 PUT에 사용자 비밀번호를 이용해 로그인할 것을 요구함

### 3.3.5 POST

* 서버에 입력 데이터를 전송 \(ex. HTML 폼\)
* 데이터는 서버로 전송되고, 서버는 이를 모아서 필요로 하는 곳\(게이트웨이 등\)에 보냄

> 정리한 사람 주: PUT과 POST의 차이는 요청 URL

### 3.3.6 TRACE

* 요청이 방화벽, 프락시, 게이트웨이 등 여러 애플리케이션을 통과할 수 있기 때문에, 클라이언트가 자신의 요청이 뮥적지 서버에 도달했을 때 어떻게 보이는지 TRACE를 통해 알 수 있다.
* 요청은 엔티티 본문과 함께 보낼 수 없으며, 응답에는 서버가 받은 요청이 그대로 들어 있다.
* loopback 진단
  * 마지막 단계에 있는 서버는 자신이 받은 요청 메시지를 본문에 넣어 응답
  * 클라이언트는 자신과 목적지 서버 사이에 있는 모든 HTTP 애플리케이션의 요청/응답 연쇄를 따라감
  * 프락시나 다른 애플리케이션이 요청에 어떤 영향을 미치는지 확인
* 문제점: 중간 애플리케이션들이 TRACE 요청을 일관되게 다루지 않을 수 있음

### 3.3.7 OPTIONS

* 리소스에 접근하지 않고 서버에게 특정 리소스에 대해 어떤 메서드가 지원되는지 물어봄

### 3.3.8 DELETE

* 서버에게 요청 URL의 리소스를 삭제할 것을 요청
* HTTP 명세는 서버가 클라이언트에게 알리지 않고 요청을 무시하는 것을 허용함. 따라서 삭제가 보장되지는 않는다.

### 3.3.9 확장 메서드

* HTTP/1.1 명세에 정의되지 않은 메서드
* ex. WebDav HTTP 확장: LOCK\(리소스 잠금\), MKCOL\(문서 생성\), COPY\(복사\), MOVE\(이동\)
* "엄격하게 보내고 관대하게 받아들여라"
  * 프락시는 알려지지 않은 메서드가 담긴 메시지도 다운스트림 서버로 전달한다.
  * 해당 메서드가 end-to-end에 영향을 끼친다면, 501 Not Implemented 상태 코드로 응답한다.

## 3.4 상태 코드

**대표적인 상태 코드**

* 200 OK: 성공 요청한 모든 데이터가 응답 본문에 들어 있다.
* 401 Unauthorized: 사용자 이름과 비밀번호를 입력해야 한다.
* 404 Not Found: 서버가 요청 URL에 해당하는 리소스를 찾지 못했다.

**다섯 가지 분류**

| 전체 범위 | 정의된 범위 | 분류 |
| :--- | :--- | :--- |
| 100-199 | 100-101 | 정보 |
| 200-299 | 200-206 | 성공 |
| 300-399 | 300-305 | 리다이렉션 |
| 400-499 | 400-415 | 클라이언트 에러 |
| 500-599 | 500-505 | 서버 에러 |

* 인식할 수 없는 상태 코드를 받게 되면 그것이 포함되는 범위의 일부라고 가정해야 함
* ex. 515는 다른 5XX 메시지들과 마찬가지로 서버 에러를 의미할 것

### 3.4.1 100-199: 정보성 상태 코드

* HTTP/1.1에서 도입되었으며 감수할 만한 가치가 있는지 논란이 있음

| 상태 코드 | 사유 구절 | 의미 |
| :--- | :--- | :--- |
| 100 | Continue | 요청의 시작 부분 일부가 받아들여졌음 |
| 101 | Switching Protocols | 서버가 프로토콜을 클라이언트가 Upgrade 헤더에 나열한 것들 중 하나로 바꿈 |

#### 100 Continue

* 엔티티 본문을 서버가 받아들일 것인지 확인할 때 사용
* 클라이언트는 100 Continue를 받고 나머지 요청을 계속 이어 보내야 함
* 서버는 100 Continue를 보낸 이후 요청을 받아 응답해야 함

#### 클라이언트와 100 Continue

* 클라이언트는 첫 번째 요청의 헤더에 `Expect: 100-continue`를 보내야 함
* 최적화: 서버가 다루거나 사용할 수 없는 큰 엔티티를 보내지 않기 위함
* 클라이언트는 서버가 100-continue 응답을 내려주기를 마냥 기다리면 안 되고, 약간의 타임아웃 후에 그냥 엔티티를 보내야 함
* 클라이언트는 예상하지 못한 100 Continue 응답에도 대비해야 함

#### 서버와 100 Continue

* 요청 헤더에 `Expect: 100-continue`가 있는 경우에만 100 Continue 응답을 주어야
* 서버가 응답을 보내기 전 엔티티의 일부 혹은 전체를 받았다면, 100 Continue를 생략하고 해당 요청에 대한 응답만 보내면 됨
* 100 Continue 요청을 받고 본문을 읽기 전 요청을 끝내기로 결정했을 때, 응답을 보내고 연결을 닫아 버리면 클라이언트는 응답을 받을 수 없게 됨 \(cf. 4장 TCP 끊기와 리셋 에러\)

#### 프락시와 100 Continue

프락시가 100 Continue 요청을 받았을 때,

* 다음 홉 서버가 HTTP/1.1을 따르거나, 어떤 버전을 따르는지 모른다면, 요청을 다음으로 전달
* 다음 홉 서버가 HTTP/1.1 이전 버전이라면 417 Expecation Failed 에러로 응답

> [홉 \(네트워크\)](https://ko.wikipedia.org/wiki/%ED%99%89_%28%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%29)

### 3.4.2 200-299: 성공 상태 코드

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC0C1;&#xD0DC; &#xCF54;&#xB4DC;</th>
      <th style="text-align:left">&#xC0AC;&#xC720; &#xAD6C;&#xC808;</th>
      <th style="text-align:left">&#xC758;&#xBBF8;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">200</td>
      <td style="text-align:left">OK</td>
      <td style="text-align:left">&#xC694;&#xCCAD; &#xC815;&#xC0C1;, &#xC5D4;&#xD2F0;&#xD2F0; &#xBCF8;&#xBB38;&#xC5D0;
        &#xC694;&#xCCAD; &#xB9AC;&#xC18C;&#xC2A4; &#xD3EC;&#xD568;</td>
    </tr>
    <tr>
      <td style="text-align:left">201</td>
      <td style="text-align:left">Created</td>
      <td style="text-align:left">&#xC0DD;&#xC131; &#xC694;&#xCCAD;(ex.PUT)&#xC5D0; &#xB300;&#xD574; &#xB9AC;&#xC18C;&#xC2A4;&#xB97C;
        &#xC0DD;&#xC131;&#xD568;
        <br />&#xD5E4;&#xB354;&#xC5D0;&#xB294; &#xC0DD;&#xC131;&#xB41C; &#xB9AC;&#xC18C;&#xC2A4;&#xC758;
        &#xAD6C;&#xCCB4;&#xC801;&#xC778; &#xCC38;&#xC870;(Location &#xD5E4;&#xB354;)
        &#xD3EC;&#xD568;
        <br />&#xC5D4;&#xD2F0;&#xD2F0; &#xBCF8;&#xBB38;&#xC5D0;&#xB294; &#xB9AC;&#xC18C;&#xC2A4;&#xB97C;
        &#xCC38;&#xC870;&#xD560; &#xC218; &#xC788;&#xB294; &#xC5EC;&#xB7EC; URL
        &#xD3EC;&#xD568;</td>
    </tr>
    <tr>
      <td style="text-align:left">202</td>
      <td style="text-align:left">Accepted</td>
      <td style="text-align:left">
        <p>&#xC694;&#xCCAD;&#xC740; &#xC815;&#xC0C1;&#xC774;&#xB098; &#xC11C;&#xBC84;&#xB294;
          &#xC5B4;&#xB5A4; &#xB3D9;&#xC791;&#xB3C4; &#xC218;&#xD589;&#xD558;&#xC9C0;
          &#xC54A;&#xC74C;</p>
        <p>(&#xC694;&#xCCAD;&#xC744; &#xB2E4;&#xB978; &#xD504;&#xB85C;&#xC138;&#xC2A4;&#xC5D0;&#xC11C;
          &#xCC98;&#xB9AC;&#xD558;&#xAC70;&#xB098;, &#xC11C;&#xBC84;&#xAC00; &#xB2E4;&#xB978;
          &#xC77C;&#xC744; &#xD558;&#xACE0; &#xC788;&#xAE30; &#xB54C;&#xBB38;)
          <br
          />&#xC5D4;&#xD2F0;&#xD2F0; &#xBCF8;&#xBB38;&#xC5D0; &#xC694;&#xCCAD;&#xC5D0;
          &#xB300;&#xD55C; &#xC0C1;&#xD0DC;&#xC640; &#xAC00;&#xAE09;&#xC801;&#xC774;&#xBA74;
          &#xC5B8;&#xC81C; &#xC644;&#xB8CC;&#xB420; &#xC218; &#xC788;&#xB294;&#xC9C0;&#xC5D0;
          &#xB300;&#xD55C; &#xCD94;&#xC815; &#xD3EC;&#xD568;&#xD574;&#xC57C; &#xD568;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">203</td>
      <td style="text-align:left">Non-Authoritative Information</td>
      <td style="text-align:left">&#xC5D4;&#xD2F0;&#xD2F0; &#xD5E4;&#xB354;&#xC5D0; &#xB4E4;&#xC5B4;&#xC788;&#xB294;
        &#xC815;&#xBCF4;&#xAC00; &#xC6D0;&#xB798; &#xC11C;&#xBC84;&#xAC00; &#xC544;&#xB2CC;
        &#xC911;&#xAC1C;&#xC790;&#xC758; &#xB9AC;&#xC18C;&#xC2A4; &#xC0AC;&#xBCF8;&#xC5D0;&#xC11C;
        &#xC634;
        <br />&#xC911;&#xAC1C;&#xC790;&#xB85C;&#xBD80; &#xB9AC;&#xC18C;&#xC2A4; &#xC0AC;&#xBCF8;&#xC744;
        &#xBC1B;&#xC558;&#xC9C0;&#xB9CC;, &#xB9AC;&#xC18C;&#xC2A4; &#xD5E4;&#xB354;&#xAC00;
        &#xC11C;&#xBC84;&#xC640; &#xC77C;&#xCE58;&#xD558;&#xC9C0; &#xC54A;&#xB294;
        &#xACBD;&#xC6B0;</td>
    </tr>
    <tr>
      <td style="text-align:left">204</td>
      <td style="text-align:left">No Content</td>
      <td style="text-align:left">&#xBCF8;&#xBB38;&#xC73C;&#xB85C; &#xBCF4;&#xB0B4;&#xC904; &#xAC83;&#xC740;
        &#xC5C6;&#xC9C0;&#xB9CC;, &#xD5E4;&#xB354;&#xC640; &#xC0C1;&#xD0DC;&#xC904;&#xC774;
        &#xC758;&#xBBF8; &#xC788;&#xB294; &#xACBD;&#xC6B0;
        <br />&#xC6F9;&#xBE0C;&#xB77C;&#xC6B0;&#xC800;&#xB97C; &#xC0C8; &#xBB38;&#xC11C;&#xB85C;
        &#xC774;&#xB3D9;&#xC2DC;&#xD0A4;&#xC9C0; &#xC54A;&#xACE0; &#xAC31;&#xC2E0;
        (ex. &#xD3FC; &#xB9AC;&#xD504;&#xB808;&#xC2DC;)</td>
    </tr>
    <tr>
      <td style="text-align:left">205</td>
      <td style="text-align:left">Reset Conent</td>
      <td style="text-align:left">&#xBE0C;&#xB77C;&#xC6B0;&#xC800;&#xC5D0;&#xAC8C; &#xD604;&#xC7AC; &#xD398;&#xC774;&#xC9C0;&#xC5D0;
        &#xC788;&#xB294; &#xBAA8;&#xB4E0; HTML &#xD3FC; &#xAC12;&#xC744; &#xBE44;&#xC6B0;&#xB77C;&#xACE0;
        &#xB9D0;&#xD568;</td>
    </tr>
    <tr>
      <td style="text-align:left">206</td>
      <td style="text-align:left">Partial Content</td>
      <td style="text-align:left">&#xBD80;&#xBD84; &#xD639;&#xC740; &#xBC94;&#xC704; &#xC694;&#xCCAD;&#xC774;
        &#xC131;&#xACF5;
        <br />Content-Range, Date &#xD5E4;&#xB354;&#xB97C; &#xBC18;&#xB4DC;&#xC2DC;
        &#xD3EC;&#xD568;
        <br />Etag/Content-Location &#xD5E4;&#xB354; &#xC911; &#xD558;&#xB098;&#xB3C4;
        &#xBC18;&#xB4DC;&#xC2DC; &#xD3EC;&#xD568;</td>
    </tr>
  </tbody>
</table>> cf. [ETag](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/ETag)

### 3.4.3 300-399: 리다이렉션 상태 코드

* 리소스가 옮겨진 사실과 어디서 찾을 수 있는지\(Location 헤더, optional\)를 알려줌
* 리다이렉트될 URL에 대한 링크와 설명을 포함하면 좋음
* 클라이언트의 복사본이 여전히 최신인지, 원래 서버에 있는 리소스가 수정되었는지 검사

| 상태 코드 | 사유 구절 | 의미 |
| :--- | :--- | :--- |
| 300 | Multiple Choices | 동시에 여러 리소스를 가리키는 URL을 요청한 경우, 리소스 목록과 함께 반환함 |
| 301 | Moved Permanently | 요청한 URL이 Location 헤더에 있는 URL로 옮겨짐 |
| 302 | Found | 301과 같으나, 클라이언트는 Location 헤더의 URL을 임시로만 사용하고 이후에는 원래 URL을 사용해야 함 |
| 303 | See Other | 리소스를 Location 헤더에 있는 URL에서 가져와야 한다고 알려줌 POST에 대한 응답으로 클라이언트에게 리소스 위치를 알려줌 |
| 304 | Not Modified | 클라이언트가 조건부 요청을 했을 때 리소스가 최근에 수정되지 않았음을 알려줌 응답은 엔티티 본문을 포함하지 않음 |
| 305 | Use Proxy | 특정 리소스에 접근할 때 Location 헤더에 있는 프록시 URL을 통해야 함 |
| 306 |  | \(현재는 사용되지 않는 코드\) |
| 307 | Temporary Redirect | 301과 비슷하나, 클라이언트는 Location 헤더의 URL을 임시로만 사용하고 이후에는 원래 URL을 사용해야 함 |

302, 303, 307은 HTTP/1.0과 1.1에서 다름

> 서버가 POST 요청을 받은 뒤, 클라이언트가 리다이렉션 URL을 받고, GET 요청으로 리다이렉트를 따라가기를 기대하는 상황이라면

* HTTP 1.0 클라이언트: 302 응답을 받고 GET 요청으로 Location 헤더에 있는 URL을 따라감
* HTTP 1.1 클라이언트: 303 응답을 받고 GET 요청으로 Location 헤더에 있는 URL을 따라감 \(307은 그대로 POST 메소드 사용할 때\)

### 3.4.4 400-499: 클라이언트 에러 상태 코드

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC0C1;&#xD0DC; &#xCF54;&#xB4DC;</th>
      <th style="text-align:left">&#xC0AC;&#xC720; &#xAD6C;&#xC808;</th>
      <th style="text-align:left">&#xC758;&#xBBF8;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">400</td>
      <td style="text-align:left">Bad Request</td>
      <td style="text-align:left">&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xAC00; &#xC798;&#xBABB;&#xB41C;
        &#xC694;&#xCCAD;&#xC744; &#xBCF4;&#xB0C4;</td>
    </tr>
    <tr>
      <td style="text-align:left">401</td>
      <td style="text-align:left">Unauthorized</td>
      <td style="text-align:left">&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xC5D0;&#xAC8C; &#xC778;&#xC99D;
        &#xC694;&#xAD6C;</td>
    </tr>
    <tr>
      <td style="text-align:left">402</td>
      <td style="text-align:left">Payment Required</td>
      <td style="text-align:left">(&#xD604;&#xC7AC; &#xC4F0;&#xC774;&#xC9C0; &#xC54A;&#xC74C;, &#xBBF8;&#xB798;&#xC5D0;
        &#xC0AC;&#xC6A9;&#xB420; &#xAC00;&#xB2A5;&#xC131;)</td>
    </tr>
    <tr>
      <td style="text-align:left">403</td>
      <td style="text-align:left">Forbidden</td>
      <td style="text-align:left">&#xC694;&#xCCAD;&#xC774; &#xC11C;&#xBC84;&#xC5D0; &#xC758;&#xD574; &#xAC70;&#xBD80;&#xB428;
        <br
        />&#xC5D4;&#xD2F0;&#xD2F0; &#xBCF8;&#xBB38;&#xC5D0; &#xAC70;&#xBD80; &#xC774;&#xC720;&#xB97C;
        &#xC124;&#xBA85;&#xD560; &#xC218; &#xC788;&#xC73C;&#xB098;, &#xBCF4;&#xD1B5;
        &#xAC70;&#xC808;&#xC758; &#xC774;&#xC720;&#xB97C; &#xC228;&#xAE30;&#xACE0;
        &#xC2F6;&#xC744; &#xB54C; &#xC500;</td>
    </tr>
    <tr>
      <td style="text-align:left">404</td>
      <td style="text-align:left">Not Found</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xC694;&#xCCAD;&#xD55C; URL&#xC744; &#xCC3E;&#xC744;
        &#xC218; &#xC5C6;&#xC74C;</td>
    </tr>
    <tr>
      <td style="text-align:left">405</td>
      <td style="text-align:left">Method Not Allowed</td>
      <td style="text-align:left">&#xC694;&#xCCAD; URL&#xC5D0; &#xB300;&#xD574; &#xC9C0;&#xD558;&#xC9C0;
        &#xC54A;&#xB294; &#xBA54;&#xC11C;&#xB4DC;&#xB85C; &#xC694;&#xCCAD;&#xBC1B;&#xC74C;</td>
    </tr>
    <tr>
      <td style="text-align:left">406</td>
      <td style="text-align:left">Not Acceptable</td>
      <td style="text-align:left">&#xC8FC;&#xC5B4;&#xC9C4; URL&#xC5D0; &#xB300;&#xD55C; &#xB9AC;&#xC18C;&#xC2A4;
        &#xC911; &#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xAC00; &#xC694;&#xCCAD;&#xD55C;
        &#xADDC;&#xACA9;&#xC5D0; &#xB9DE;&#xB294; &#xAC83;&#xC774; &#xC5C6;&#xC74C;</td>
    </tr>
    <tr>
      <td style="text-align:left">407</td>
      <td style="text-align:left">Proxy Authentication Required</td>
      <td style="text-align:left">401&#xACFC; &#xAC19;&#xC73C;&#xB098; &#xD504;&#xB77D;&#xC2DC; &#xC11C;&#xBC84;&#xB97C;
        &#xC704;&#xD574; &#xC0AC;&#xC6A9;&#xD568;</td>
    </tr>
    <tr>
      <td style="text-align:left">408</td>
      <td style="text-align:left">Request Timeout</td>
      <td style="text-align:left">&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xC758; &#xC694;&#xCCAD;&#xC744;
        &#xC644;&#xC218;&#xD558;&#xAE30;&#xC5D0; &#xC2DC;&#xAC04;&#xC774; &#xB108;&#xBB34;
        &#xB9CE;&#xC774; &#xAC78;&#xB9BC;</td>
    </tr>
    <tr>
      <td style="text-align:left">409</td>
      <td style="text-align:left">Conflict</td>
      <td style="text-align:left">
        <p>&#xC694;&#xCCAD;&#xC774; &#xCDA9;&#xB3CC;&#xC744; &#xC77C;&#xC73C;&#xD0AC;
          &#xC5FC;&#xB824;&#xAC00; &#xC788;&#xC74C;
          <br />&#xBCF8;&#xBB38;&#xC5D0; &#xCDA9;&#xB3CC;&#xC5D0; &#xB300;&#xD574; &#xC124;&#xBA85;&#xD574;&#xC57C;
          &#xD568;</p>
        <p>ex. &#xC11C;&#xBC84;&#xC5D0; &#xC788;&#xB294; &#xD30C;&#xC77C;&#xBCF4;&#xB2E4;
          &#xC624;&#xB798;&#xB41C; &#xD30C;&#xC77C;&#xC744; PU&#xC73C;&#xB85C; &#xC5C5;&#xB85C;&#xB4DC;&#xD560;
          &#xB54C; &#xBC84;&#xC804; &#xC81C;&#xC5B4; &#xCDA9;&#xB3CC;&#xC774; &#xBC1C;&#xC0DD;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">410</td>
      <td style="text-align:left">Gone</td>
      <td style="text-align:left">404&#xC640; &#xBE44;&#xC2B7;&#xD558;&#xB098; &#xB9AC;&#xC18C;&#xC2A4;&#xB97C;
        &#xAC00;&#xC9C0;&#xACE0; &#xC788;&#xC5C8;&#xB2E4; &#xC81C;&#xAC70;&#xB41C;
        &#xACBD;&#xC6B0;</td>
    </tr>
    <tr>
      <td style="text-align:left">411</td>
      <td style="text-align:left">Length Required</td>
      <td style="text-align:left">&#xC694;&#xCCAD;&#xC5D0; Content-Length &#xD5E4;&#xB354; &#xC694;&#xAD6C;</td>
    </tr>
    <tr>
      <td style="text-align:left">412</td>
      <td style="text-align:left">Precondition Failed</td>
      <td style="text-align:left">&#xC870;&#xAC74;&#xBD80; &#xC694;&#xCCAD; &#xC911; &#xD558;&#xB098;&#xAC00;
        &#xC2E4;&#xD328;</td>
    </tr>
    <tr>
      <td style="text-align:left">413</td>
      <td style="text-align:left">Request Entity Too Large</td>
      <td style="text-align:left">&#xC694;&#xCCAD;&#xC758; &#xD06C;&#xAE30;&#xAC00; &#xC11C;&#xBC84;&#xAC00;
        &#xCC98;&#xB9AC;&#xD560; &#xC218; &#xC788;&#xB294;(or &#xCC98;&#xB9AC;&#xD558;&#xACE0;&#xC790;
        &#xD558;&#xB294;) &#xD55C;&#xACC4;&#xB97C; &#xB118;&#xC74C;</td>
    </tr>
    <tr>
      <td style="text-align:left">414</td>
      <td style="text-align:left">Request URI Too Long</td>
      <td style="text-align:left">&#xC694;&#xCCAD; URL &#xAE38;&#xC774;&#xAC00; &#xC11C;&#xBC84;&#xAC00;
        &#xCC98;&#xB9AC;&#xD560; &#xC218; &#xC788;&#xB294;(or &#xCC98;&#xB9AC;&#xD558;&#xACE0;&#xC790;
        &#xD558;&#xB294;) &#xD55C;&#xACC4;&#xB97C; &#xB118;&#xC74C;</td>
    </tr>
    <tr>
      <td style="text-align:left">415</td>
      <td style="text-align:left">Unsupported Media Type</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xC9C0;&#xC6D0;&#xD558;&#xC9C0; &#xC54A;&#xB294;
        &#xC720;&#xD615;&#xC758; &#xC5D4;&#xD2F0;&#xD2F0; &#xBCF8;&#xBB38;</td>
    </tr>
    <tr>
      <td style="text-align:left">416</td>
      <td style="text-align:left">Requested Range Not Satisfiable</td>
      <td style="text-align:left">&#xC694;&#xCCAD;&#xD55C; &#xB9AC;&#xC18C;&#xC2A4;&#xC758; &#xBC94;&#xC704;&#xAC00;
        &#xC798;&#xBABB;&#xB428;</td>
    </tr>
    <tr>
      <td style="text-align:left">417</td>
      <td style="text-align:left">Expectation Failed</td>
      <td style="text-align:left">Expect &#xC694;&#xCCAD; &#xB354;&#xC5D0; &#xC788;&#xB294; &#xB0B4;&#xC6A9;&#xC744;
        &#xC11C;&#xBC84;&#xAC00; &#xB9CC;&#xC871;&#xC2DC;&#xD0AC; &#xC218; &#xC5C6;&#xC74C;</td>
    </tr>
  </tbody>
</table>### 3.4.5 500-599: 서버 에러 상태 코드

* 클라이언트에서 올바른 요청을 보냈지만 서버에서 에러 발생

| 상태 코드 | 사유 구절 | 의미 |
| :--- | :--- | :--- |
| 500 | Server Error | 서버가 요청을 처리할 수 없음 |
| 501 | Not Implemented | 클라이언트가 서버가 구현하지 않은 메서드로 요청을 함 |
| 502 | Bad Gateway | 프락시나 게이트웨이 등 다음 링크로부터 부정적인 응답을 받음 |
| 503 | Service Unavailabe | 현재는 서버가 요청을 처리해줄 수 없지만 나중에는 가능함 Retry-After 헤더에 언제 가능한지를 포함하기도 함 |
| 504 | Gateway Timeout | 408과 비슷하지만 게이트웨이나 프록시가 보내는 응답 |
| 505 | HTTP Version Not Supported | 요청 프로토콜이 서버가 지원할 수 없는 버전임 |

## 3.5 헤더

* &lt;이름&gt; &lt;콜론\(:\)&gt; \(&lt;공백&gt;\) &lt;값&gt; &lt;CRLF&gt;가 순서대로 나타나는 0개 이상의 헤더들
* HTTP/1.1과 같은 몇몇 버전의 HTTP는 요청이나 응답에 특정 헤더가 포함되어야만 유효한 것으로 간주

### 3.5.1 일반 헤더

* 클라이언트, 서버 모두 사용
* 메시지에 대한 기본적 정보 제공

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD5E4;&#xB354;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Connection</td>
      <td style="text-align:left">&#xD604;&#xC7AC;&#xC758; &#xC804;&#xC1A1;&#xC774; &#xC644;&#xB8CC;&#xB41C;
        &#xD6C4; &#xB124;&#xD2B8;&#xC6CC;&#xD06C; &#xC811;&#xC18D;&#xC744; &#xC720;&#xC9C0;&#xD560;&#xC9C0;
        &#xB9D0;&#xC9C0;&#xB97C; &#xC81C;&#xC5B4;</td>
    </tr>
    <tr>
      <td style="text-align:left">Date</td>
      <td style="text-align:left">&#xBA54;&#xC2DC;&#xC9C0;&#xAC00; &#xB9CC;&#xB4E4;&#xC5B4;&#xC9C4; &#xB0A0;&#xC9DC;&#xC640;
        &#xC2DC;&#xAC04;</td>
    </tr>
    <tr>
      <td style="text-align:left">MIME-Version</td>
      <td style="text-align:left">
        <p>MIME &#xBC84;&#xC804; (&#xD604;&#xC7AC; 1.0)</p>
        <p>&#xC2E4;&#xC9C8;&#xC801;&#xC73C;&#xB85C;&#xB294; &#xD574;&#xB2F9; &#xBA54;&#xC2DC;&#xC9C0;&#xAC00;
          MIME &#xD615;&#xC2DD;&#xC784;&#xC744; &#xB098;&#xD0C0;&#xB0C4;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Trailer</td>
      <td style="text-align:left">&#xC778;&#xCF54;&#xB529;&#xB41C; (chunked) &#xBA54;&#xC2DC;&#xC9C0;&#xC758;
        &#xB05D; &#xBD80;&#xBD84;&#xC5D0; &#xCD94;&#xAC00; &#xD5E4;&#xB354;&#xB4E4;&#xC758;
        &#xBAA9;&#xB85D;</td>
    </tr>
    <tr>
      <td style="text-align:left">Transfer-Encoding</td>
      <td style="text-align:left">&#xC5B4;&#xB5A4; &#xC778;&#xCF54;&#xB529;&#xC774; &#xC801;&#xC6A9;&#xB418;&#xC5C8;&#xB294;&#xC9C0;</td>
    </tr>
    <tr>
      <td style="text-align:left">Upgrade</td>
      <td style="text-align:left">&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xAC00; &#xC5C5;&#xADF8;&#xB808;&#xC774;&#xB4DC;&#xD558;&#xAE38;
        &#xC6D0;&#xD558;&#xB294; &#xC0C8; &#xBC84;&#xC804;&#xC774;&#xB098; &#xD504;&#xB85C;&#xD1A0;&#xCF5C;</td>
    </tr>
    <tr>
      <td style="text-align:left">Via</td>
      <td style="text-align:left">&#xD504;&#xB77D;&#xC2DC;, &#xAC8C;&#xC774;&#xD2B8;&#xC6E8;&#xC774; &#xB4F1;
        &#xBA54;&#xC2DC;&#xC9C0;&#xAC00; &#xAC70;&#xCCD0;&#xC628; &#xC911;&#xAC1C;&#xC790;</td>
    </tr>
  </tbody>
</table>#### 일반 캐시 헤더

매번 서버로부터 객체를 가져오는 대신 사본으로 캐시해주는 헤더

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD5E4;&#xB354;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Cache-Control</td>
      <td style="text-align:left">
        <p>&#xCE90;&#xC2DC; &#xC9C0;&#xC2DC;&#xC790; &#xC804;&#xB2EC;</p>
        <p>cf. <a href="https://www.zerocho.com/category/HTTP/post/5b594dd3c06fa2001b89feb9">&#xCFE0;&#xD0A4;/&#xCE90;&#xC2DC; &#xD5E4;</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Pragma</td>
      <td style="text-align:left">&#xCE90;&#xC2DC; &#xC9C0;&#xC2DC;&#xC790; &#xC804;&#xB2EC;&#xC744; &#xC704;&#xD55C;
        &#xB610; &#xB2E4;&#xB978; &#xBC29;&#xBC95;</td>
    </tr>
  </tbody>
</table>### 3.5.2 요청 헤더

* 요청 메시지에 사용
* 클라이언트의 선호나 능력
* 서버가 더 나은 응답을 주는 데 활용

| 헤더 | 설명 |
| :--- | :--- |
| Client-IP | 클라이언트 IP |
| From | 클라이언트 사용자의 메일 주소 |
| Host | 요청의 대상이 되는 서버의 호스트 명과 포트 |
| Referer | 요청 URI가 들어있었던 문서의 URL |
| UA-Color | 클라이언트 기기의 디스플레이 색상 능력 |
| UA-CPU | 클라이언트 기기의 CPU |
| UA-OS | 클라이언트 기기의 OS |
| UA-Pixels | 클라이언트 기기의 디스플레이 픽셀 |
| User-Agent | 요청을 보낸 애플리케이션의 이름 |

#### Accept 관련 헤더

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD5E4;&#xB354;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Accept</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xBCF4;&#xB0B4;&#xB3C4; &#xB418;&#xB294; &#xBBF8;&#xB514;&#xC5B4;
        &#xC885;&#xB958;</td>
    </tr>
    <tr>
      <td style="text-align:left">Accept-Charset</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xBCF4;&#xB0B4;&#xB3C4; &#xB418;&#xB294; &#xBB38;&#xC790;
        &#xC9D1;&#xD569;</td>
    </tr>
    <tr>
      <td style="text-align:left">Accept-Encoding</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xBCF4;&#xB0B4;&#xB3C4; &#xB418;&#xB294; &#xC778;&#xCF54;&#xB529;</td>
    </tr>
    <tr>
      <td style="text-align:left">Accept-Language</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xBCF4;&#xB0B4;&#xB3C4; &#xB418;&#xB294; &#xC5B8;&#xC5B4;</td>
    </tr>
    <tr>
      <td style="text-align:left">TE</td>
      <td style="text-align:left">
        <p>&#xC11C;&#xBC84;&#xAC00; &#xBCF4;&#xB0B4;&#xB3C4; &#xB418;&#xB294; &#xD655;&#xC7A5;
          &#xC804;&#xC1A1; &#xCF54;&#xB529;</p>
        <p>cf. <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/TE">TE</a> (en)</p>
        <p><a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Transfer-Encoding">transfer encodings</a>(ko)</p>
      </td>
    </tr>
  </tbody>
</table>#### 조건부 요청 헤더

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD5E4;&#xB354;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Expect</td>
      <td style="text-align:left">&#xC694;&#xCCAD;&#xC5D0; &#xD544;&#xC694;&#xD55C; &#xC11C;&#xBC84;&#xC758;
        &#xD589;&#xB3D9;</td>
    </tr>
    <tr>
      <td style="text-align:left">If-Match</td>
      <td style="text-align:left">
        <p>&#xBB38;&#xC11C;&#xC758; &#xC5D4;&#xD2F0;&#xD2F0; &#xD0DC;&#xADF8;(ETag)&#xAC00;
          &#xC8FC;&#xC5B4;&#xC9C4; &#xC5D4;&#xD2F0;&#xD2F0; &#xD0DC;&#xADF8;&#xC640;
          &#xC77C;&#xCE58;&#xD574;&#xC57C; &#xD568;</p>
        <p>cf. <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/ETag">ETag</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">If-None-Match</td>
      <td style="text-align:left">&#xBB38;&#xC11C;&#xC758; &#xC5D4;&#xD2F0;&#xD2F0; &#xD0DC;&#xADF8;&#xAC00;
        &#xC8FC;&#xC5B4;&#xC9C4; &#xC5D4;&#xD2F0;&#xD2F0; &#xD0DC;&#xADF8;&#xC640;
        &#xC77C;&#xCE58;&#xD558;&#xC9C0; &#xC54A;&#xC544;&#xC57C; &#xD568;</td>
    </tr>
    <tr>
      <td style="text-align:left">If-Modified-Since</td>
      <td style="text-align:left">&#xC8FC;&#xC5B4;&#xC9C4; &#xB0A0;&#xC9DC; &#xC774;&#xD6C4;&#xC5D0; &#xB9AC;&#xC18C;&#xC2A4;&#xAC00;
        &#xBCC0;&#xACBD;&#xB418;&#xC5C8;&#xC744; &#xB54C;&#xB9CC; &#xC694;&#xCCAD;
        (&#xBCC0;&#xACBD;&#xB418;&#xC9C0; &#xC54A;&#xC558;&#xC73C;&#xBA74; &#xC81C;&#xD55C;)</td>
    </tr>
    <tr>
      <td style="text-align:left">If-Unmodified-Since</td>
      <td style="text-align:left">&#xC8FC;&#xC5B4;&#xC9C4; &#xB0A0;&#xC9DC; &#xC774;&#xD6C4;&#xC5D0; &#xB9AC;&#xC18C;&#xC2A4;&#xAC00;
        &#xBCC0;&#xACBD;&#xB418;&#xC9C0; &#xC54A;&#xC558;&#xC744; &#xB54C;&#xB9CC;
        &#xC694;&#xCCAD; (&#xBCC0;&#xACBD;&#xB418;&#xC5C8;&#xC73C;&#xBA74; &#xC81C;&#xD55C;)</td>
    </tr>
    <tr>
      <td style="text-align:left">If-Range</td>
      <td style="text-align:left">&#xBB38;&#xC11C;&#xC758; &#xBC94;&#xC704;&#xC5D0; &#xB300;&#xD55C; &#xC694;&#xCCAD;</td>
    </tr>
    <tr>
      <td style="text-align:left">Range</td>
      <td style="text-align:left">&#xB9AC;&#xC18C;&#xC2A4;&#xC5D0; &#xB300;&#xD55C; &#xD2B9;&#xC815; &#xBC94;&#xC704;&#xB97C;
        &#xC694;&#xCCAD;</td>
    </tr>
  </tbody>
</table>#### 요청 보안 헤더

| 헤더 | 설명 |
| :--- | :--- |
| Authorization | 인증 자체에 대한 정보 \(ex. `username:pw`\) |
| Cookie | 클라이언트가 서버에게 토큰 전달 보안 헤더는 아니지만 보안에 영향을 줌 |
| Cookie2 | 클라이언트가 지원하는 쿠키의 버전 |

#### 프락시 요청 헤더

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD5E4;&#xB354;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Max-Forwards</td>
      <td style="text-align:left">
        <p>&#xC694;&#xCCAD;&#xC774; &#xD504;&#xB77D;&#xC2DC;&#xB098; &#xAC8C;&#xC774;&#xD2B8;&#xC6E8;&#xC774;&#xB85C;
          &#xC804;&#xB2EC;&#xB420; &#xC218; &#xC788;&#xB294; &#xCD5C;&#xB300; &#xD69F;&#xC218;
          <br
          />TRACE &#xBA54;&#xC11C;&#xB4DC;&#xC640; &#xD568;&#xAED8; &#xC0AC;&#xC6A9;&#xB428;</p>
        <p>cf. <a href="http://withbundo.blogspot.com/2017/08/http-15-http-v-max-forwards-proxy.html">Max-Forwards, Proxy-Authorization, Referer, TE</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Proxy-Authorization</td>
      <td style="text-align:left">Authorization&#xACFC; &#xAC19;&#xC73C;&#xB098; &#xD504;&#xB77D;&#xC2DC;&#xC5D0;&#xC11C;
        &#xC778;&#xC99D;&#xD560; &#xB54C; &#xC4F0;&#xC784;</td>
    </tr>
    <tr>
      <td style="text-align:left">Proxy-Connection</td>
      <td style="text-align:left">Connection&#xACFC; &#xAC19;&#xC73C;&#xB098; &#xD504;&#xB77D;&#xC2DC;&#xC5D0;&#xC11C;
        &#xC5F0;&#xACB0;&#xC744; &#xB9FA;&#xC744; &#xB54C; &#xC4F0;&#xC784;</td>
    </tr>
  </tbody>
</table>### 3.5.3 응답 헤더

* 응답 메시지에 사용

| 헤더 | 설명 |
| :--- | :--- |
| Age | 응답이 얼마나 오래되었는지 초단위의 시간 보통 0이나 락시와 같은 중개자를 거쳐왔을 때는 있 |
| Public | 특정 리소스에 대해 지원하는 메서드 목록 |
| Retry-After | 리소스가 현재 사용 불가능할 때 언제 가능해지는지 |
| Server | 서버 애플리케이션의 이름과 버전 |
| Title | HTML 문서 제목 |
| Warning | 자세한 경고 메시지 |

#### 협상 헤더

* 서버에 여러 가지 표현이 가능한 문서가 있을 때, 서버와 클라이언트가 그 중 어떤 표현을 택할 것인지 협상 지원
* ex. 서버에 한국어, 영어로 번역된 HTML 문서가 있을 때

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xD5E4;&#xB354;</th>
      <th style="text-align:left">&#xC124;&#xBA85;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Accept-Ranges</td>
      <td style="text-align:left">&#xBD80;&#xBD84; &#xC694;&#xCCAD;&#xC744; &#xD560; &#xB54C; &#xC11C;&#xBC84;&#xAC00;
        &#xC790;&#xC6D0;&#xC5D0; &#xB300;&#xD574; &#xBC1B;&#xC544;&#xB4E4;&#xC77C;
        &#xC218; &#xC788;&#xB294; &#xBC94;&#xC704; &#xB2E8;&#xC704;&#xC758; &#xD615;&#xD0DC;
        (ex. bytes)</td>
    </tr>
    <tr>
      <td style="text-align:left">Vary</td>
      <td style="text-align:left">
        <p>&#xC751;&#xB2F5;&#xC5D0; &#xC601;&#xD5A5;&#xC744; &#xC904; &#xC218; &#xC788;&#xC5B4;&#xC11C;
          &#xC11C;&#xBC84;&#xAC00; &#xD655;&#xC778;&#xD574; &#xBCF4;&#xC544;&#xC57C;
          &#xD558;&#xB294; &#xD5E4;&#xB354;&#xB4E4;&#xC758; &#xBAA9;&#xB85D;</p>
        <p>cf. <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Vary">Vary</a>
        </p>
      </td>
    </tr>
  </tbody>
</table>#### 응답 보안 헤더

| 헤더 | 설명 |
| :--- | :--- |
| Proxy-Authenticate | 프락시에서 클라이언트로 보낸 인증 요구의 목록 |
| Set-Cookie | 클라이언트에 토큰을 설정하기 위해 사용 보안 헤더는 아니지만 보안에 영향을 줌 |
| Set-Cookie2 | Set-Cookie와 비슷하되 RFC 2965로 정의됨 |
| WWW-Authenticate | 서버에서 클라이언트로 보낸 인증 요구의 목록 |

### 3.5.4 엔티티 헤더

* 엔티티 본문에 대한 헤더
* 요청과 응답 메시지 모두에 나타날 수 있음

| 헤더 | 설명 |
| :--- | :--- |
| Allow | 엔티티에 대해 수행될 수 있는 메서드 목록 |
| Location | 리소스의 \(새로운\) URL |

#### 콘텐츠 헤더

| 헤더 | 설명 |
| :--- | :--- |
| Content-Base | 본문의 상대 URL에 대한 기저 URL |
| Content-Encoding | 본문에 적용된 인코딩 |
| Content-Language | 본문의 언어\(자연어\) |
| Content-Length | 본문 길이 |
| Content-Location | 리소스 위치 |
| Content-MD5 | MD5 checksum |
| Content-Range | 전체 리소스에서 해당 엔티티의 범위 \(바이트\) |
| Content-Type | 본문의 종류 |

#### 엔티티 캐싱 헤더

* 언제 어떻게 캐시가 되어야 하는지, 캐시된 사본이 유효한지, 리소스가 유효하지 않게 되는 시점이 언제인지 등

| 헤더 | 설명 |
| :--- | :--- |
| ETag | 엔티티 태그 |
| Expires | 엔티티가 유효하지 않게 되는 일시 |
| Last-Modified | 엔티티가 최근 변경된 일시 |

cf. 확장 헤더

* HTTP 명세에 없는 헤더
* HTTP 애플리케이션은 확장 헤더의 의미를 몰라도 용인하고 전달할 수 있어야 함

