# multiple-apps-proxy

![Reverse Proxy Diagram](/img/proxy.drawio.svg)

A reverse proxy handles incoming client requests, and then forwards those requests to another server that runs in the backend. This demo showcases using nginx as a reverse proxy to host multiple apps running under different paths of the same domain.


### Example:
- `domain.com` -> APP #1
- `domain.com/api` -> APP #2


### To run the demo:
- you'll need [Docker](https://www.docker.com/)
- check out the repository
- run `docker-compose up`

This will spin up thee containers:
- the reverse proxy server on [http://localhost:80](http://localhost:80)
- an nginx site on [http://localhost:8001](http://localhost:8001) serving the files from the folder `./app_web`
- an apache site on [http://localhost:8002](http://localhost:8002) serving the files from the folder `./app_api`

The reverse proxy is configured in `./nginx.conf`. 
- Requests to [http://localhost:80](http://localhost:80) will be routed to the Nginx server.  
- Requests to [http://localhost:80/api](http://localhost:80/api) will be routed to the Apache server.  

### Conderations around the base path / url rewrite:
Since the request path is `/api`, the Apache site would by default respond with the file at `/app_api/api/index.html`. Depending on the inner workings of the app in question, that might be the desired behavior. Apps which are able to be configured so that they know they live on a subdirectory of a domain dramatically reduce the complexity of using multiple apps next to each other (e.g. [NextJS](https://nextjs.org/docs/api-reference/next.config.js/basepath), [NestJS](https://docs.nestjs.com/faq/global-prefix)).

On the other hand, some apps might want to treat the request to `/api` as the request to the root of the app in question (e.g. static sites). In that case the request can be rewritten in the reverse proxy config. For demo purposes, this is the current implementation in the example `./nginx.conf`. It is important to watch out for possibly overlapping paths with this approach (e.g. both apps having a static folder and assuming it lives under `/static/`).
