# Fast loading HTML pages

1. Reduce page weight: remove comments, unnecasary white space
2. Minimize number of files. 
   1. Browser sends an `If-Modified-Since` header to ask for a file that has been modified since the last time it was downloaded. Too much time spent reading this can increase first render time. 
   2. Reduce CSS files by combining them into sprites
3. Use CDN: CDN is a means to reduce the physical distance between your server and your visitor. Use CDN to store cached versions of website and serve them to visitors closest to the user.
4. Reduce domain lookup
5. Cached reused content   
   1. Pay attention to `Last-Modified` header. It allows for efficient page caching. 
   2. Send HTTP 304 if `Last-Modified` and `ETag` are same. Etags are assigned with the resource representation of URL changes. When a URL is retreived, the web server will return the resoruces's current representation along with its corresponding ETag value, which is placed in the HTTP header field. The client may decide to cache the representation along with ETag value. Later, if the client wants to retrieve the same URL resource again, it will first determine whether the locally cached version of the URL has expired ( through the Cache-Control and Expire headers). If the URL has not expired, it will retrieve the locally cached resource. If it is determined that the URL is expired, the client will contact the server and send its previously-saved copy of the ETag along with the request in an "If-None-Match" field. On the subsequent request, the server may now compare the client's Etag with the Etag of current version of the resource. If the Etag values match, nmeaning that the resource has not changed, the server sends back an `HTTP 304 Not Modifed` status. 
6. Optimally order the components of the page. Download page content first, along with CSS or Js, download dynamic content after
7. Reduce the number of inline scripts. Reduce the use of document.write() ,instead use AJAX. Inline scripts can be expensive since the parser must assume that an inline script could modify the page structure while parsing is in progress. 
8. Use modern CSS and valid markup.
9. Chunk your content. Tables for layouts are a legacy method that should not be used anymore. Tables should be utilized for tabular data. Avoid nesting tables. 
10. Minimize and compress SVG assets.
11. Minimize and compress images. 
12. Specify sizes for images and tables. If the browser can immeditately determine your height and width of images it will be able to display a web page without having the reflow the content. always specifiy `height` and `width` for imags. Tables should use CSS selector `table-layout: fixed`
13. Use lazy loady for images. By default, images are loaded **eagerly**. All eagerly loaded images are rendered before the document's load event is sent. To mark an image for lazy loading, specify the `loading` attribute to `lazy`. You can determine if a given image is loaded by checking to see if the value of its Boolean `complete` property is true. 
14. Choose your user-agent requirements wisely. Make sure that reasoanble user-agent requirements are specified for projects. D not require your content to appear pixel-perfect in all browsers, especially not in down-version browsers. 
15. Use `async` and `defer` is possible. 
    1.  If `async` attribute is present, then the classic script will be fetched in parallel to parsing and evaluated as soon as it is available. For module scripts, if the `async` attribute is present then the scripts and all their dependencies will be executed in the defer queue, therefore they will get fetched in parallel to parsing and evaluated as soon as they are available. This attribute allows the elimination of `parser-blocking JS`.
    2. `defer` is set to indicate that the script is meant to be executed after the document has been parsed, but before firing `DOMContentLoaded` event. Scripts with `defer` will prevent the `DOMContentLoaded` event from firing until the script has loaded and finished evaluating.  
    