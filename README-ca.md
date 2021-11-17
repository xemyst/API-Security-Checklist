[繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md) | [Ελληνικά](./README-el.md) | [Català](./README-ca.md) 

# LLista de seguretat en APIs
LLista de les contramesures més importants en el que respecta al disseny, testeig i llençament de les teves APIs.

---

## Autenticació
- [ ] No utilitzis `Basic Auth`. Utilitza els estandards d'autenticació (e.g. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] No reinventis la roda en `Autenticació`, `generació de tokens`, `enmagatzemament de contrasenyes`. Utilitza els estandards.
- [ ] Utilitza una politica de limit de intents  (`Max Retry`) y funcionalitats de jailing al Login.
- [ ] Utilitza encryptació en totes les dades sensibles.

### JWT (JSON Web Token)
- [ ] Utilitza una clau complexa i aleatoria (`JWT Secret`) per dificultar al màxim els atacs de força bruta.
- [ ] No extreguis el algoritme del contingut. Força l'algoritme al backend (`HS256` o `RS256`).
- [ ] Fes que l'expiració del token (`TTL`, `RTTL`) sigui el més curta possible.
- [ ] No guardis informació sensible al contingut del JWT, pot ser decodificada [facilemnt](https://jwt.io/#debugger-io).

### OAuth
- [ ] Comprova sempre `redirect_uri` al costat del servidor per permetre solament certes URLs.
- [ ] Intenta intercanviar sempre codi i no tokens (no permetis `response_type=token`).
- [ ] Utilitza el parametre `state` amb un hash aleatori per previndre CSRF al procés d'autenticació OAuth.
- [ ] Defineix el àmbit (`scope`) per defecte, i valideu els parametres del àmbit per a cada aplicació.

## Accés
- [ ] Limit requests (Throttling) to avoid DDoS / brute-force attacks.
- [ ] Use HTTPS on server side to avoid MITM (Man in the Middle Attack).
- [ ] Use `HSTS` header with SSL to avoid SSL Strip attack.
- [ ] For private APIs, only allow access from whitelisted IPs/hosts.

## Entrades
- [ ] Use the proper HTTP method according to the operation: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, and `DELETE (to delete a record)`, and respond with `405 Method Not Allowed` if the requested method isn't appropriate for the requested resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g. `application/xml`, `application/json`, etc.) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc.).
- [ ] Validate user input to avoid common vulnerabilities (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc.).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use an API Gateway service to enable caching, Rate Limit policies (e.g. `Quota`, `Spike Arrest`, or `Concurrent Rate Limit`) and deploy APIs resources dynamically.

## Processament
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource ID should be avoided. Use `/me/orders` instead of `/user/654321/orders`.
- [ ] No utlitzis IDs auto incrementals. Utilitza `UUID` en el seu lloc.
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Utilitza un CDN per la pujada de fitxers.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to process as much as possible in background and return response fast to avoid HTTP Blocking.
- [ ] No oblidis de deshabilitar el mode de DEBUG.

## Sortides
- [ ] Envia la capcelera `X-Content-Type-Options: nosniff`.
- [ ] Envia la capcelera `X-Frame-Options: deny`.
- [ ] Envia la capcelera `Content-Security-Policy: default-src 'none'`.
- [ ] Elimina les capceleres que deixen petjades - `X-Powered-By`, `Server`, `X-AspNet-Version`, etc.
- [ ] Força `content-type` a les teves respostes. Si retornes un `json`, llavors el teu `content-type` serà `application/json`.
- [ ] No retornis dades sensibles com poden ser: `credencials`, `Contrasenyes`, o `Tokens de seguretat`.
- [ ] Retorna el codi HTTP corresponent a la operació completada. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc.).

## CI & CD
- [ ] Audita el teu disseny i implemetació utilitzatzant el testeig unitari i d'integració i `test coverage`.
- [ ] Utilitza processos de revisió de codi i evita les auto-aprovacions.
- [ ] Asegura que tots els components dels teus serveis son escanejats estaticaments utilitzant un software AV abans d'anar a producció, incloient llibreries de tercers i dependències.
- [ ] Dissenya un process de `rollback` per als teus desplegaments.


---

## Veure També:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Una colecció de recursos útils per a la creació d'APIs RESTful HTTP+JSON.


---

# Contribució
Sentiu-vos lliures de contrubuir fent un "fork" d'aquest repository, fent alguns canvis, i sotmeten una solicitud de "pull". Per a qualsevol pregunta envieu-nos un email a `team@shieldfy.io`.
