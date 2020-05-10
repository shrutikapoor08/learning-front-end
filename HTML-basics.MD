# `<head>`

1. `<meta>` : used to describe data. 
    1. charset - "utf-8" used to describe document's char encoding - the char set that the document is permitted to use. with utf-8, the document is able to handle displaying any language. `ISO-8859-1` is for example Latin.
    2. name - eg, author, description
    3. content
    4. og:image, og:description, og:title: used to describe "open graph data" that helps in displaying custom urls on facebook. similarly twitter:title. 
 1. `link`: stylesheet, icons
 2. `script`: The script tag does not have to go in head but modern browsers will load faster this way. 


#`<body>`
1. Difference between `<i>`, `<em>` and `<b>` `<strong>`. The former are presentational tags, the latter are semantic tags. Presentational tags should no longer be used since semantics is so important to accessibility, SEO, etc. 
2. `<video>` : `src, controls, p (fallback content)`
3. `<iframe>`: used to embed other web documents into the current document. Great for third-party content in your website. Never embed content with `iframe` in HTTP, always use HTTPS. To prevent malicious users from executing JS on the rest of the page, you can prevent access by using `sandbox` attribute in the embdedded content. For content security policy, you can send an appropriate `X-Frame-Options` header which prevents other websites from embedding your content in their web pages. 


#### Note on async and defer
`async` and `defer`: defer tells the browser that it shouldnt run the script immediately but rather after it finished reading the document. without this the browser will have to stop and wait for script to be downloaded, whereeas placing the script in the head advises the browser early about all the remote scripts and where they are hosted. Both practices together gives the browser all the information needed for efficient load. 
Using `async` attribute instead of defer is an even more radical performance hint but should be used with caution. It will mostly only be safe to use if the script is optional. async should be used for scripts that are safe to run at any time, including both "before the page has all of its elements" and "after the user is allowed to interact with page". 


#### Note on responsive images
1. Resolution switching: A technique where a smaller picture is served for smaller devices. `srcset` and `sizes` to pick between multiple images, eg: `sizes: (max-width: 600px)`. 
2. Art direction problem: The problem where a picture on a smaller screen loses the focus of the main subjects. Use `<picture>` to fix this. 


##### Attribution
* Lessons learnt and most content from: 
https://developer.mozilla.org/en-US/docs/


