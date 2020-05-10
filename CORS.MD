# CORS - Cross Origin Resource Sharing

## Same origin policy 
Same origin policy is a security mechanism that helps dictate how a script on one origin can interact with a resource from another origin. 

## Cross-origin network access rules. 
1. Cross origin writes are allowed. eg: links, redirects, form submissions
2. Cross origin embedding is allowed. 
3. Cross origin reads are disallowed, but read access is often leaked by embedding. Following embedding is allowed cross-origin.
   1. JS with `<script>`. Syntax errors are only available for same-origin scripts. 
   2. CSS with `<link>`. cross-origin requires a correct `Content-Type` header. 
   3. `<img>`, `<video>`, `<audio>`
   4. Fonts with `@font-face`
   5. `<iframe>`. Sites can use `X-Frame-Options` to prevent cross-origin framing.


### Simple requests
Requests that meet **all** the following conditions - 
1. One of `GET`, `HEAD`, `POST`
2. Apart from the headers automatically sent by the uder agent, the only other headers sent are the ones that are defined in Fetch spec as "CORS-safelisted request-header" - 
   1. `Accept`
   2. `Accept-Language`
   3. `Content-Language`
   4. `Content-Type`
   5. `DPR`
   6. `Downlink`
   7. `Save-Data`
   8. `Viewport-Width`
   9. `Width`
3.  `Content-Type` can only be 
    1.  `application/x-www-form-urlencoded`
    2.  `multipart/form-data`
    3.  `text/plain`
4. No event listeners are registered on `XMLHttpRequestUpload`
5. No `ReadableStream` object is used. 


## Allowing CORS
CORS is a mechanism that allows using additional HTTP headers to tell browsers to give a web app running at one origin, access to another origin's resources. Eg: an XHR request from `https://mysite.com` to `https://mynewdomain.com`. `XHR` and `FetchAPI` **follow same-origin policy**. 

The `Origin` and **`Access-Control-Allow-Origin`** header show access control set by the server. If `Access-Control-Allow-Origin` is set to `*`, it is allowed to be accessed on any domain. 

**Pre-flight request**
Preflight requests use the `HTTP OPTIONS` method and include `Access-Control-Request` headers to ask the server what kind of CORS requests are allowed. The server's response tells the browser whether your main request can be sent.

To allow a cross-origin request, you need to first send a pre-flighted request.
1. Include `OPTIONS` to determine if the request is safe to send. 

``` 
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: http://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type 

```

The `Access-Control-Request-Method` tells that when the actual request is sent, it will be sent as a `POST`. 

2.  The server will respond back with information to show what is allowed, `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`

```
HTTP/1.1 204 No Content
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```

Then, the real request is sent - 

```
POST /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
X-PINGOTHER: pingpong
Content-Type: text/xml; charset=UTF-8
Referer: https://foo.example/examples/preflightInvocation.html
Content-Length: 55
Origin: https://foo.example
Pragma: no-cache
Cache-Control: no-cache
```

If preflight request is not possible by the browser, to get past it - 
1. Make a `simple request` using `response.url` to determine what URL the preflight will end up at. 
2. Make a "real request" to the preflight url 

## Blocking CORS
1. To prevent cross origin writes, you can use **CSRF (Cross-Site Request Forgery)** token. You must prevent cross origin reads of pages that require this token. 
2. Make content non-embeddable since embedding a content often leaks some information. 

## Cross-origin API access
JS APIs like `iframe.contentWindow`, `window.parent` allow documents to reference each other. When they are not in the same origin, `Window` and `Location` objects will provide very limited access. 

** To communicate between documents from various origins, use `window.postMessage`. **



## Cross-origin data storage access
1. Browser storage: Access to data stored in WebStorage is limited to same origin documents. 
2. Cookies: a page can set cookie for its own domain or any parent domain. The browser will make a cookie available to the given domain including any sub-domains. 


## TODO
1. Requests with credentials using ` Access-Control-Allow-Credentials: true`


##### Attribution
* Content from: 
https://developer.mozilla.org/en-US/docs/