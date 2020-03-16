# 질문과 답변

## Q. hreflang

> wikipedia [https://en.wikipedia.org/wiki/Hreflang](https://en.wikipedia.org/wiki/Hreflang) 구글 검색결과에 페이지의 언어/지역 설정하기 [https://support.google.com/webmasters/answer/189077](https://support.google.com/webmasters/answer/189077)

### 1. HTTP 헤더

* 페이지에 GET 응답을 했을 때, 언어 및 지역 버전을 알려준다.
* PDF와 같이 HTML이 아닌 파일에 유용하다. \(HTML은 3번 방법을 쓸 수 있기 때문인 듯하다.\)

```text
Link: <url1>; rel="alternate"; hreflang="{lang_code_1}",
<url2>; rel="alternate"; hreflang="{lang_code_2}", ...
```

_url\_x_ 해당 언어/지역 페이지의 URL.

_lang\_code\_x_ 이 버전에서 지원되는 언어/지역 코드

예시:

```text
Link: <http://example.com/file.pdf>; rel="alternate"; hreflang="en",
      <http://de-ch.example.com/file.pdf>; rel="alternate"; hreflang="de-ch",
      <http://de.example.com/file.pdf>; rel="alternate"; hreflang="de"
```

### 2. 사이트맵

* 각 URL마다 별도의  요소를 만든다.
* 각  요소 하위에  하위 요소를 만든다.
* 각  요소 하위에  하위 요소를 만든다.

예시: \(영어, 독일어, 스위스 독일어 세 개 페이지 있을 때\)

```markup
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
  xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>http://www.example.com/english/page.html</loc>
    <xhtml:link 
               rel="alternate"
               hreflang="de"
               href="http://www.example.com/deutsch/page.html"/>
    <xhtml:link 
               rel="alternate"
               hreflang="de-ch"
               href="http://www.example.com/schweiz-deutsch/page.html"/>
    <xhtml:link 
               rel="alternate"
               hreflang="en"
               href="http://www.example.com/english/page.html"/>
  </url>
  <url>
    <loc>http://www.example.com/deutsch/page.html</loc>
    <xhtml:link 
               rel="alternate"
               hreflang="de"
               href="http://www.example.com/deutsch/page.html"/>
    <xhtml:link 
               rel="alternate"
               hreflang="de-ch"
               href="http://www.example.com/schweiz-deutsch/page.html"/>
    <xhtml:link 
               rel="alternate"
               hreflang="en"
               href="http://www.example.com/english/page.html"/>
  </url>
  <url>
    <loc>http://www.example.com/schweiz-deutsch/page.html</loc>
    <xhtml:link 
               rel="alternate"
               hreflang="de"
               href="http://www.example.com/deutsch/page.html"/>
    <xhtml:link 
               rel="alternate"
               hreflang="de-ch"
               href="http://www.example.com/schweiz-deutsch/page.html"/>
    <xhtml:link 
               rel="alternate"
               hreflang="en"
               href="http://www.example.com/english/page.html"/>
  </url>
</urlset>
```

### 3. 페이지 헤더

* 사이트맵이 없거나, 사이트에 HTTP 응답 헤더를 지정할 수 없는 경우 사용한다.
* `<head>` 안에 `<link>` 집합을 넣는다. link 집합은 현재 페이지의 다른 모든 버전의 집합이다.
* 각 버전마다 링크가 있어야 한다. 링크 집합은 페이지의 모든 버전에서 동일해야 한다.

```markup
<link rel="alternate" hreflang="{lang_code}" href="{url_of_page}" />
```

_lang\_code_

* 페이지의 이 버전에서 타겟팅하는 지원되는 언어/지역 코드
* 또는 페이지의 hreflang 태그에 의해 명시적으로 나열되지 않은 모든 언어에 해당하는 x-default

_url\_of\_page_ 이 페이지의 지정된 언어/지역별 버전의 정규화된 URL

예시:

```markup
<head>
  <title>Widgets, Inc</title>
  <link rel="alternate" hreflang="en-gb"
        href="http://en-gb.example.com/page.html" />
  <link rel="alternate" hreflang="en-us"
        href="http://en-us.example.com/page.html" />
  <link rel="alternate" hreflang="en"
        href="http://en.example.com/page.html" />
  <link rel="alternate" hreflang="de"
        href="http://de.example.com/page.html" />
  <link rel="alternate" hreflang="x-default"
        href="http://www.example.com/" />
</head>
```

## Q. 웹의 크로스플랫폼 환경 지원 \(Mobile 인지 Desktop 인지\)도 내용 협상을 통해 결정 할 수 있을 것 같다

### **1. Vary 헤더에 UserAgent 를 넣는다?**

좋지 않은 방법임.

* UserAgent에 의지하면
  * 이와 대응하는 수 많은 응답이 필요함
  * 또는 문자열을 계산해서 표준화시켜야 함
* 서버는 모든 UserAgent에 대응할 수 없다..
  * \(=**의도한대로 캐싱이 잘 안될 수 있다**\)
  * UserAgent는 통신 SW의 이름과 그의 버전 및 기타 상세정보\(comment\) 등 너무 많은 정보를 포함한다.
    * 또는 규칙을 위반하거나 지워진 UserAgent

**그래도 사용하는 경우**

> [MDN :: Browser\_detection\_using\_the\_user\_agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Browser_detection_using_the_user_agent#Mobile_Device_Detection) 에서는 Navigator.maxTouchPoints 를 사용해서 모바일을 감지하기를 권장하지만

* 위 요소\(maxTouchPoints\) 를 브라우저가 지원하지 않거나 \(fallback 으로 이용\)
* 브라우저에 심각한 버그가 있어 대응해야 하는 경우 UserAgent 를 활용한다.

### **2. Accept-CH 헤더에 값을 넣는다.**

**Experimental, chrome 46+ 에서만.......**

1. DPR \(클라이언트 기기 픽셀 비율\)
2. Viewport-WIdth \(CSS 픽셀에서의 레이아웃 뷰포트\)
3. Width \(물리적인 픽셀에서의 리소스 너비\)

#### Q. 모바일 컨텐츠 \(크로스플랫폼 컨텐츠\) 를 위한 협상을 피하기 위해 \(대안?\)

**일단 협상은 최소한으로 했으면 좋겠다.**

* 서버 주도 협상으로,
* 또는 프록시 주도로,
* 가능한 `Accept, Accept-Charset, Accept-Encoding, Accept-Language` 만 이용해서
* **아니면 협상하지 않는다**

  ![https://user-images.githubusercontent.com/20185848/75695173-02ab3280-5ced-11ea-99b6-489cb149caa2.png](https://user-images.githubusercontent.com/20185848/75695173-02ab3280-5ced-11ea-99b6-489cb149caa2.png)

  * 협상을 한다는 것 : 협상 과정에서 리소스 응답시간 증가
  * 협상 유연하게 하면 할 수록 : 캐시가 관리해야 하는 요청 가지 수 증가

[2006년에 이미 w3.org 에서는](https://www.w3.org/blog/2006/02/content-negotiation/) 언어 협상 또한 안 좋은 사용자 경험을 줄 수 있음이 기고됐다.

* 굳이 불필요한 협상은 모바일 컨텐츠가 아니더라도 좋지 않다.
* `브라우저가 원하는 언어` 가 아니라 `사용자가 원하는 언어` 를 제공해야 한다.
  * 사용자는 브라우저 언어 세팅이 어떻게 이루어지는지 모를 수 있다
* 요즘 웹 사이트는 결국 최종에 가서는 사용자와 협상한다. \(사용자가 웹에서 언어를 선택함\)

결론 : 모바일의 경우에는 대안으로 [반응형 웹](https://developers.google.com/web/fundamentals/design-and-ux/responsive?hl=ko) 을 고려해보면 어떨지?

> 애초에 이런 복잡한 구성을 지양하려고 협상을 하는 것 같은데... 요즘 와서는 이런 기술들에 대한 지원이 많고 자료도 방대하므로 괜찮을 듯 하다

그럼 Vary 헤더는 언제 쓰는거고... 어떻게 써야 잘 썼다고 소문날까...

### Vary 헤더 모범사례 \(번역 및 요약\)

> [https://www.fastly.com/blog/best-practices-using-vary-header](https://www.fastly.com/blog/best-practices-using-vary-header)

#### 캐시를 사용하지 않는 경우

Vary:\* 사용 \(절대 하지 마라. 캐시를 사용하지 않는 것이 과연 당신이 원하는 것인가??\)

#### UserAgent, Cookie 등을 사용하여 협상하고 싶은 경우

캐시에 들어오기 전에 별도의 요청 헤더에 전처리하여 추가한다.

예를들어 UserAgent 에 다음과 같이 특정 문구가 들어있는 경우

```text
if    req.http.User-Agent ~ "(Mobile|Android|iPhone|iPad)"
set   req.http.User-Agent = "mobile";
```

User-Agent 를 전처리 한다.

