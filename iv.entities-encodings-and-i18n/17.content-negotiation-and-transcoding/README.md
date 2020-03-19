# 17. 내용 협상과 트랜스코딩

* HTTP provides content-negotiation methods that allow clients and servers to make determination about which version of the document to send by the server, English or French.
* A single URL can correspond to different resources.
* These different versions are called variants.
* Servers can automatically generate customized pages.
  * WML
  * transcodings.

## Content-Negotiation techniques

* three distinct methods for deciding which page at a server is the right one for a client.
* present the choice to the client - client-driven negotiation
  * client makes a request, server sends list of choices to client, client chooses.
  * easiest to implement.
  * adds latency.
* decide automatically at the server - server-driven negotiation
  * server examines request headers and decide what version to serve.
  * q-value mechanism, `Vary` header
  * server might have to guess.
* ask an intermediary to select - transparent negotiation
  * a proxy cache does the request negotiation.
  * offloads the negotiation from the server.
  * no formal specifications.

## Client-Driven Neogitation

* server sends back a response listing the available pages and let the client decide.
* easiest to implement at the server.
* two requests are needed.
* two ways for server to present the choices
  * sending back HTML document with links to the different versions of the page.
  * 300 Multiple Choices
* Client may popup a dialog window for a user to choose.
* This method requires multiple URLs.
  * `www.joes-hardware.com/english`, `www.joes-hardware.com/french`

## Server-Driven Negotation

* let the server decide which page to send back.
* two mechanisms that HTTP servers use to evaluate the proper response
  * examine content-negotiation headers
    * `Accept` headers.
  * Varying on other headers.
    * `User-Agent` header

### Content-Negotiation Headers

* `Accept` - media types
* `Accept-Language` - languages
* `Accept-Charset` - charsets
* `Accept-Encoding` - encodings
* similar to entity headers.
* Content-negotiation headers are used by clients and servers
  * to exchange preference information
  * to help choose between different versions of a document.
* clients must send their preference information with every request.
* reduces latency with the back-and-forth communication in client-driven model.
* Spaniard requests, then English or French?
  * guess or fall back to client-driven.
  * you can use _quality values_\(q values\)

### Content-Negotiation Header Quality Values

* `Accept-Language: en;q=0.5, fr;q=0.0, nl;q=1.0, tr;q=0.0`
* q values can range from 0.0 to 1.0
* the order are not important, only q values are.
* server may change or transcode the document to match the client's preference.
  * Q. google translate

### Varing on Other Headers

* Server can attempt to match up responses with other client request such as `User-Agent`
  * remove JavaScript from the page if not supports
* Caches must attempt to serve correct "best" versions of cached documents
  * `Vary` hjeader tells caches which headers the server is using to determine the best version.

### Content Negotiation on Apache

## Transparent Negotiation

* intermediary proxy negotiate on behalf of client.
  * move the load of server-driven negotiation away from the server.
  * minimize message exchanges with the client.
* server must be able to tell proxies what request headers the server examines - `Vary` header.
* Caching proxies can store different copies of documents accessed via a single URL.
  * the caches can negotiate with clients on behalf of the servers.
  * also great place to transcode content - can trascode content from any server.
    * Q - modern web examples?

### Caching and Alternates

* caches must employ much of the decision-making logic that servers do.
  * choose correct document to serve.
* `Accept` headers to decide which cached response to send back.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6af8bcf0-593b-488b-902f-3c9a5048a893/Screen\_Shot\_2020-03-01\_at\_12.29.38\_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6af8bcf0-593b-488b-902f-3c9a5048a893/Screen_Shot_2020-03-01_at_12.29.38_PM.png)

**Figure 17-1**

* Cache should check `Accept-Language` header to send back correct response.
* if cache doesn't have spanish version, should forward the second request to server, and store both the response and an "alternate" response for that URL.
* two different documents for the same URL - `variants` or `alternates`

### The Vary Header

* Q - Accept 헤더의 앞부분에 있을 수록 우선순위가 높은 것일까?
* if servers are using other headers to make their decisions, caches must know.
* the `Vary` header lists all of the client request headers that server considers.
  * if served document depends on `User-Agent`, `Vary` must include it.
* before cache can serve document to the client, it must see whether the server sent a `Vary` header in the cached response.
  * if present, header values in the new request must match the header values in the old, cached request.
* caches must store both the client request headers and the corresponding server response headers with each cached variant.
* `Vary: User-Agent, Cookie`
  * the number of `User-Agent` and `Cookie` values could generate many variants.
  * cache would have to store each document version corresponding to each variant.

## transcoding

* when a server does not have a document that matches the client's needs at all?
* may have to respond with an error, but may be able to transform one of its existing documents into something that client can use. _transcoding_

### Format conversion

* transformation of data from one format to another to make it viewable by client.
* HTML-[WML](https://en.wikipedia.org/wiki/Wireless_Markup_Language)
* different from content encoding or transfer encoding.
  * latter two are used for more efficient or safe transport of content.
  * the former is used to make content viewable on the access device.

### Information Synthesis

* the extraction of key pieces of information from a document.
* generation of an outline of a document based on section headings, removal of advertisements, and logos from a page.
* categorizing pages based on keywords in content
* Q. vs og-tags?
* Q. modern web technologies to handle these?

### Content Injection

* may increase the amount of content.
* automatic ad generators, user-tracking systems.
  * has to be dynamic - be relevant, target for a particular user.

### Transcoding Versus Static Pregeneration

* build different copies of web apges at the web server.
* not a very practical technique
  * any small change in a page requires multiple pages to be modified.
  * more psace is necessary to all the different versions.
  * harder to catalog pages.
  * ad-insertion cannot be done statistically.
    * increase latency in serving content - computation for on-the-fly transformation.
    * can be done by external agent\(proxy, cache\)
* Q. vs SSR?

## Next Steps

* performance limits
  * searching through many variants for appropriate content.
  * trying to guess the best match.
* straming media and fax
  * client and server need to discuss the best answer to the client's request. - not HTTP.
  * can a general content-negotiation protocol be developed on top of TCP/IP application protocols?

## ref

* [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept)
* [https://en.wikipedia.org/wiki/Wireless\_Markup\_Language](https://en.wikipedia.org/wiki/Wireless_Markup_Language)
* [preflight request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request)

