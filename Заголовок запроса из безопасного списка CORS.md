
# Заголовок запроса из безопасного списка CORS

[Заголовок запроса из безопасного списка CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS-safelisted_request_header)


Заголовок [запроса, внесенный в безопасный список CORS,](https://fetch.spec.whatwg.org/#cors-safelisted-request-header) — это один из следующих [заголовков HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) :

-   [`Accept`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept),
-   [`Accept-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language),
-   [`Content-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language),
-   [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type).

Если запрос содержит только эти заголовки (и значения, соответствующие дополнительным требованиям, изложенным ниже), в контексте [CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS) не требуется отправлять предварительный [запрос](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) .[подробнее](https://developer.mozilla.org/en-US/docs/Glossary/CORS)

Вы можете внести в безопасный список больше заголовков, используя [`Access-Control-Allow-Headers`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)заголовок, а также перечислить там заголовки, указанные выше, чтобы обойти дополнительные ограничения.



#cors