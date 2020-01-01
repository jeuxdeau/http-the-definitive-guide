# 11. 클라이언트 식별과 쿠키

### 11.1 개별 접촉

#### HTTP의 특

1. 익명이다.
2. 상태가 없다.
3. 요청과 응답으로 통신한다.

\(1\) 사용자를 식별하거나, \(2\) 연속적인 요청을 추적하기 위해서는, 약간의 정보가 필요하.

약간의 정보를 이용한다.

#### 개인화 예

* 개별 인사 \(ex. 우빈 님, 안녕하세요?\)
* 사용자 맞춤 추천
* 주소, 신용카드 정보 입력 자동화 \(cf. DB에 저장하기도 함\)
* 세션 추적 \(ex. 장바구니\)

### 11.2 HTTP 헤더

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC774;&#xB984;</th>
      <th style="text-align:left">&#xD0C0;</th>
      <th style="text-align:left">&#xC124;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">From</td>
      <td style="text-align:left">&#xC694;&#xCCAD;</td>
      <td style="text-align:left">
        <p>ex. From: woobin@linecorp.com</p>
        <ul>
          <li>&#xB85C;&#xBD07;, &#xC2A4;&#xD30C;&#xC774;&#xB354;&#xB294; &#xC6F9;&#xB9C8;&#xC2A4;&#xD130;&#xAC00;
            &#xD56D;&#xC758; &#xBA54;&#xC77C;&#xC744; &#xBCF4;&#xB0BC; &#xC218; &#xC788;&#xB3C4;&#xB85D;
            &#xBA54;&#xC77C; &#xC8FC;&#xC18C;&#xB97C; &#xAE30;&#xC7AC;</li>
          <li>&#xC545;&#xC758;&#xC801;&#xC778; &#xC11C;&#xBC84; &#xC774;&#xBA54;&#xC77C;
            &#xC8FC;&#xC18C;&#xB97C; &#xBAA8;&#xC544;&#xC11C; &#xC2A4;&#xD338; &#xBA54;&#xC77C;&#xC744;
            &#xBC1C;&#xC1A1;&#xD558;&#xAE30;&#xB3C4; &#xD568;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">User-Agent</td>
      <td style="text-align:left">&#xC694;&#xCCAD;</td>
      <td style="text-align:left">
        <p>ex. User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X)
          ...</p>
        <ul>
          <li>&#xBE0C;&#xB77C;&#xC6B0;&#xC800; &#xC774;&#xB984;, &#xBC84;&#xC804; &#xC815;&#xBCF4;
            (+ &#xC6B4;&#xC601;&#xCCB4;&#xC81C; &#xC815;&#xBCF4;)</li>
          <li>&#xCF58;&#xD150;&#xCE20;&#xB97C; &#xCD5C;&#xC801;&#xD654;&#xD558;&#xB294;
            &#xB370; &#xC720;&#xC6A9;</li>
          <li>User-Agent&#xB85C; &#xD2B9;&#xC815; &#xC0AC;&#xC6A9;&#xC790;&#xB97C; &#xC2DD;&#xBCC4;&#xD560;
            &#xC218;&#xB294; &#xC5C6;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Referer</td>
      <td style="text-align:left">&#xC694;&#xCCAD;</td>
      <td style="text-align:left">
        <p>ex. referer: <a href="https://app.gitbook.com/">https://app.gitbook.com/</a>
        </p>
        <ul>
          <li>&#xC0AC;&#xC6A9;&#xC790;&#xAC00; &#xD604;&#xC7AC; &#xD398;&#xC774;&#xC9C0;&#xB85C;
            &#xC720;&#xC785;&#xD558;&#xAC8C; &#xD55C; &#xC6F9;&#xD398;&#xC774;&#xC9C0;&#xC758;
            URL</li>
          <li>&#xC6F9; &#xC0AC;&#xC6A9; &#xD589;&#xD0DC; &#xBD84;&#xC11D;</li>
          <li>Referer&#xB85C; &#xD2B9;&#xC815; &#xC0AC;&#xC6A9;&#xC790;&#xB97C; &#xC2DD;&#xBCC4;&#xD560;
            &#xC218;&#xB294; &#xC5C6;&#xC74C;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Authorization</td>
      <td style="text-align:left">&#xC694;&#xCCAD;</td>
      <td style="text-align:left">&#xC0AC;&#xC6A9;&#xC790; &#xC774;&#xB984;&#xACFC; &#xBE44;&#xBC00;&#xBC88;&#xD638;
        (cf. 11.4)</td>
    </tr>
    <tr>
      <td style="text-align:left">Client-ip</td>
      <td style="text-align:left">&#xD655;&#xC7A5;(&#xC694;&#xCCAD;)</td>
      <td style="text-align:left">&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xC758; IP &#xC8FC;&#xC18C; (cf.
        11.3)</td>
    </tr>
    <tr>
      <td style="text-align:left">X-Forwarded-For</td>
      <td style="text-align:left">&#xD655;&#xC7A5;(&#xC694;&#xCCAD;)</td>
      <td style="text-align:left">
        <p>X-Forwarded-For: &lt;client&gt; &lt;proxy1&gt; &lt;proxy2&gt; &#xD615;&#xC2DD;
          <br
          />ex. X-Forwarded-For: 203.0.113.195, 70.41.3.18, 150.172.238.178</p>
        <ul>
          <li>&#xD074;&#xB77C;&#xC774;&#xC5B8;&#xD2B8;&#xC758; IP &#xC8FC;&#xC18C; (cf.
            11.3)</li>
          <li>cf. &#xD45C;&#xC900;&#xD654;&#xB41C; &#xBC84;&#xC804;&#xC740; <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Forwarded">Forwarded</a> &#xD5E4;&#xB354;</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Cookie</td>
      <td style="text-align:left">&#xD655;&#xC7A5;(&#xC694;&#xCCAD;)</td>
      <td style="text-align:left">&#xC11C;&#xBC84;&#xAC00; &#xC0DD;&#xC131;&#xD55C; ID &#xB77C;&#xBCA8;
        (cf. 11.6)</td>
    </tr>
  </tbody>
</table>> 우빈이의 질문: 어디서도 Client-ip 헤더에 대한 정보를 찾을 수 없었다.

### 11.3 클라이언트 IP 주소

* 사용자 식별에 클라이언트 IP 주소 사용 \(ex. 인트라넷에서 특정 IP 주소에만 응답\)
* 보통 HTTP 헤더에 IP 주소가 들어가지는 않음
* 웹 서버 HTTP 요청을 보내 반대쪽 TCP 커넥션의 IP 주소를 알아냄

#### IP 주소의 문제점

1. IP 주소는 사용자가 아니라 컴퓨터를 가리킨다. 여러 사용자가 같은 컴퓨터를 사용할 수 있다.
2. ISP는 동적으로 IP 주소를 할당한다.
3. 많은 사용자가 NAT 방화벽을 통해 인터넷을 사용한다. NAT 장비들은 클라이언트들의 실제 IP 주소를 하나의 방화벽 IP 주소로 변환한다.
4. 서버가 프락시나 게이트웨이와 연결되어 있는 경우, 서버는 클라이언트의 IP 주소 대신 프락시의 IP 주소를 본다. \(프락시 확장 헤더\(Client-ip, X-Forwarded-For\)를 사용할 수도 있지만 모든 프락시가 그렇게 하지는 않는다.\)

### 11.4 사용자 로그인

1. 클라이언트가 요청
2. 서버는 401 Login Required 응답과 WWW-Authenticate 헤더를 반환해 로그인 요청
3. 사용자가 입력한 로그인 정보 토큰 Authorization 헤더에 담아 보냄 \(ex. `Authorization: Basic am910jRmdW4=`\)
4. 추후 요청에 대해서는 서버가 요청하지 않아도 사용자 이름과 비밀번호를 포함해 전달해 세션이 진행되는 내내 사용자에 대한 식별을 유지함

#### 사용자 로그인의 문제점

* 모든 사이트에 로그인하는 게 귀찮음
* 서로 다른 사용자 이름과 비밀번호를 기억해야 함

### 11.5 뚱뚱한 URL

1. 사용자가 처음 방문하면 유일한 ID가 생성됨
2. URL 처음이나 끝에 ID를 추가함 ex. `http://amazon.com/.../`**`002-1145265-801683`**
3. 서버가 ID를 포함한 요청을 받으면 사용자와 관련된 추가적인 정보\(ex. 장바구니, 프로필\)를 찾고, 응답을 뚱뚱한 URL로 만

#### 뚱뚱한 URL의 문제점

* URL이 못생겨서 사용자 혼란
* 공유 불가: 공유하면 뚱뚱한 URL에 담긴 개인정보가 같이 공유됨
* 캐시 접근 불가: URL이 계속 달라지기 때문에 캐시를 사용할 수 없음
* 서버 부하 증가: 뚱뚱한 URL에 해당하는 각 페이지들을 새로 그려야 함
* 세션 지속 불가: 사용자가 뚱뚱한 URL을 북마크하지 않는 이상 로그아웃하면 모든 정보를 잃
* 사용자가 세션에서 이탈하기 쉬움

