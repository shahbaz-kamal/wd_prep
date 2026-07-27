# 3. HTTP & APIs

**Methods:** GET (safe, idempotent), POST (not idempotent), PUT (idempotent, full replace), PATCH (partial, not necessarily idempotent), DELETE (idempotent), HEAD, OPTIONS (CORS preflight)

**Status codes**
- 1xx informational
- 2xx: 200 OK, 201 Created, 202 Accepted, 204 No Content
- 3xx: 301 Moved Permanently, 302 Found, 304 Not Modified
- 4xx: 400 Bad Request, 401 Unauthorized (not authenticated), 403 Forbidden (authenticated, not allowed), 404 Not Found, 405 Method Not Allowed, 409 Conflict, 422 Unprocessable Entity, 429 Too Many Requests
- 5xx: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout

**Concepts**
- HTTP is stateless. State via cookies, sessions, tokens.
- HTTP/1.1 keep-alive; HTTP/2 multiplexing + binary framing + header compression; HTTP/3 over QUIC/UDP.
- REST constraints: client-server, stateless, cacheable, uniform interface, layered, code-on-demand (optional).
- REST vs GraphQL: multiple endpoints/over-fetching vs single endpoint/client-specified queries.
- Idempotent = same result if repeated. Safe = no state change.
- WebSocket = full-duplex persistent connection, starts as an HTTP upgrade.
