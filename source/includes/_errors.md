# Errors

<!-- <aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside> -->

Our API uses the following error codes:

| Error Code | Meaning                                                                                     |
| ---------- | ------------------------------------------------------------------------------------------- |
| 400        | Bad Request -- Your request is invalid, please check the payload is correct.                |
| 401        | Unauthorized -- Your API key is invalid.                                                    |
| 403        | Forbidden -- You don't have access to that object.                                          |
| 404        | Not Found -- The object could not be found.                                                 |
| 405        | Method Not Allowed -- You tried to used a HTTP method not available on that route.          |
| 429        | Too Many Requests                                                                           |
| 500        | Internal Server Error -- We had a problem with our server. Try again later (or contact us)! |
| 503        | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.   |
