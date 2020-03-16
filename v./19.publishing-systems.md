# 19. 배포 시스템

* 먼 과거: 메모장에서 HTML 개발함 FTP를 통해 웹 서버에 콘텐츠를 올림
* 가까운 과거: 콘텐츠를 화면에서 보면서 작성함 클릭으로 배포함 \(FrontPage, DAV\)

### 19.1 FrontPage

**용어 정리**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC6A9;&#xC5B4;</th>
      <th style="text-align:left">&#xB73B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">FrontPage &#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;</td>
      <td style="text-align:left">FP&#xC758; &#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8; &#xCE21; &#xC18C;&#xD504;&#xD2B8;&#xC6E8;&#xC5B4;</td>
    </tr>
    <tr>
      <td style="text-align:left">FrontPage &#xC11C;&#xBC84; &#xD655;&#xC7A5; (FPSE)</td>
      <td style="text-align:left">FP&#xC758; &#xC11C;&#xBC84; &#xCE21; &#xC18C;&#xD504;&#xD2B8;&#xC6E8;&#xC5B4;</td>
    </tr>
    <tr>
      <td style="text-align:left">FP RPC &#xD504;&#xB85C;&#xD1A0;&#xCF5C;</td>
      <td style="text-align:left">
        <p>FP &#xC11C;&#xBC84;&#xC640; &#xD074;&#xB77C;&#xAC00; &#xD1B5;&#xC2E0;&#xC5D0;
          &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xD504;&#xB85C;&#xD1A0;&#xCF5C;.</p>
        <p>HTTP POST &#xC694;&#xCCAD; &#xC704;&#xC5D0; &#xAD6C;&#xD604;&#xB41C; RPC
          &#xACC4;&#xCE35;&#xC774;&#xB2E4;.</p>
        <p>&#xC989;, &#xC11C;&#xBC84;&#xB294; FP &#xD504;&#xB85C;&#xD1A0;&#xCF5C;&#xB85C;
          POST &#xC694;&#xCCAD;&#xC744; &#xBC1B;&#xC544; &#xCC98;&#xB9AC;&#xD55C;&#xB2E4;.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#xAC00;&#xC0C1; &#xC11C;&#xBC84;</td>
      <td style="text-align:left">
        <p>&#xC6F9; &#xC11C;&#xBC84; &#xD55C; &#xAC1C;&#xC5D0;&#xC11C; &#xC6F9; &#xC0AC;&#xC774;&#xD2B8;
          &#xC5EC;&#xB7EC; &#xAC1C;&#xB97C; &#xD638;&#xC2A4;&#xD305;&#xD55C; &#xAC83;.</p>
        <p>&#xAC00;&#xC0C1; &#xC11C;&#xBC84;&#xB97C; &#xC9C0;&#xC6D0;&#xD558;&#xB294;
          &#xC11C;&#xBC84;: &#xB2E4;&#xC911; &#xD638;&#xC2A4;&#xD305;(multi-hosting)
          &#xC11C;
          <br />cf. &#xC5EC;&#xB7EC; &#xAC1C;&#xC758; IP &#xC8FC;&#xC18C;&#xB85C; &#xAD6C;&#xC131;&#xB41C;
          &#xC7A5;&#xBE44;: &#xB2E4;&#xC911;-&#xD648;(multi-homed) &#xC11C;&#xBC84;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#xB8E8;&#xD2B8; &#xC6F9;</td>
      <td style="text-align:left">&#xC6F9; &#xC11C;&#xBC84;&#xC758; &#xCD5C;&#xC0C1;&#xC704; &#xB514;&#xB809;&#xD130;&#xB9AC;</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC11C;&#xBE0C; &#xC6F9;</td>
      <td style="text-align:left">&#xB8E8;&#xD2B8; &#xC6F9;&#xC758; &#xD558;&#xC704; &#xB514;&#xB809;&#xD130;&#xB9AC;</td>
    </tr>
  </tbody>
</table>**FrontPage의 동작** 

1. 클라이언트는 한 서버의 여러 사이트 중에서 요청을 보낼 대상을 결정해야 한다. 이를 위해 GET 요청을 보낸다.
2. GET 요청의 응답이 오면. `FPShtmlScriptUrl`, `FPAuthorScriptUrl`, `FPAdminScriptUrl`등 값을 통해 프로그램이 어디에 위치하고 있는지 알 수 있다. 
3. 클라이언트는 `method=<command>` 형식으로 RPC 명령을 보낸다.

   ```text
   method=list+documents:4.0.1.3717
   &service_name= // URL
   &listHiddenDocs=false // 숨김 파일을 보는지, 아닌지
   &listExplorerDocs=false // 리스트를 나열할지, 말
   ```

4. 서버는 특정 형식에 맞추어 응답을 보낸다. 

**FP 보안 모델**

* 보안은 서버에서 관리한다.
* 사용자를 관리자, 저작자, 브라우징하는 일반 사용자 세 등급으로 나눈다.
* 서브 웹은 루트 웹에서 권한을 상속받거나 자체 권한을 만든다.
* 설정 파일에 사용자의 권한\(GET/POST/…\), 그룹, IP 주소 등을 지정할 수 있다.

### 19.2 WebDAV와 공동 저작

* 웹 분산 저작과 버저닝\(Web Distributed Authoring and Versioning\)의 준말
* \(먼 과거에는\) 이메일을 사용하거나, 파일 공유를 해서 협업해서 버전 관리가 어려웠음

**WebDAV와 XML**

* HTTP는 헤더에만 요청, 응답 정보가 들어감
* WebDAV는 XML을 이용해 서버의 응답을 어떻게 표현할 것인지, 콜렉션과 리소스를 어떻게 처리할 것인지, 데이터 자체를 어떻게 표현할 것인지를 표현함

**WebDAV와 헤더**

여러 가지 HTTP 헤더를 추가해 WebDAV 메서드를 지원함.

**WebDAV 잠금과 덮어쓰기 방지**

잠금\(lock\): 공동 작업시 덮어쓰는 문제를 방지함. WebDAV에서는 `LOCK`과 `UNLOCK` 메서드로 지원한다.

* 배타적 쓰기 잠금: 잠금 소유자만 쓸 수 있다. 잠재적인 충돌을 완벽히 제거한다.
* 공유적 쓰기 잠금: 그룹 단위\(여러 사람이 하나의 그룹을 구성\) 하나의 문서를 쓴다.

**LOCK 과정**

1. 서버는 다이제스트 인증\(13장 참고\)으로 사용자를 식별한다.
2. 이 사용자에게 잠금이 승인되면, 서버는 클라이언트에게 토큰을 반환한다.
3. 클라이언트가 서버에 쓰기를 하고 싶으면, 서버에 연결하고 다이제스트 인증을 수행한다.
4. 인증이 완료되면 클라이언트는 PUT 요청을 통해 잠금 토큰을 보낸다.

**UNLOCK 과정**

1. 서버는 다이제스트 인증으로 사용자를 식별한다.
2. 사용자는 UNLOCK 메소드에 잠금 토큰을 실어 보낸다. 
3. 토큰이 맞으면 서버는 `204 No Content` 상태 코트를 반환한다.

**속성과 메타데이터**

`PROPFIND` 메서드로 속성 찾기, `PROPPATCH` 메서드로 속성 수정하기를 지원한다.

* live 속성: 계속 동적으로 변하는 속성 \(ex. 문서가 편집될 때 수정되는 저자\)
* dead 속성: 변하지 않는 속성 ex. Content-Type

**콜렉션과 이름공간 관리**

* 콜렉션: 리소스들의 논리적 또는 물리적 그룹. ex. 디렉터리
* WebDAV에서는 XML 이름공간\(namespace\)을 사용해서 리소스 간의 구조를 제어한다.
  * `MKCOL` 메서드: 콜렉션 생성
  * `DELETE` 메서드: 콜렉션 삭제
  * `COPY`, `MOVE`

**향상된 HTTP/1.1 메서드**

WebDAV에서는 HTTP의 `DELETE`, `PUT`, `OPTIONS`가 수정된 의미로 사용된다.

* `DELETE`: 콜렉션 삭제를 위해 사용됨
* `PUT`: 콘텐츠를 전송하기 위해 사용됨
* `OPTIONS`: 클라이언트가 서버에 첫 요청을 보낼 때 WebDAV 서버가 무엇을 지원하는지 알려줌

**WebDAV의 버전 관리**

WebDAV에 처음부터 버전 관리 개념이 있었던 것은 아니고, RFC 3253에서 추가되었다.

**WebDAV의 미래**

현재도 잘 지원되고 있다. \(이 책은 2002년에 쓰여졌다.\)

