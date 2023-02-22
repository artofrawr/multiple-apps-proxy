# multiple-apps-proxy

[Reverse Proxy Diagram](img/proxy.drawio)

A reverse proxy handles incoming client requests, and then forwards those requests to another server that runs in the backend. This demo showcases using nginx as a reverse proxy to host multiple apps running under different locations of the same domain.

** Example:**

- `domain.com` -> APP #1
- `domain.com/api` -> APP #2
