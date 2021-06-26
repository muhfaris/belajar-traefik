## Config
```
defaultEntryPoints = ["http"]

# enable dashboard
[api]
    dashboard = true
    #insecure = true

# create entrypoint
[entryPoints]
    [entryPoints.http]
        address = ":8888"
    [entryPoints.api]
        address = ":8080"

# user file provider
[providers]
    [providers.file]
        filename = "traefik-dynamic.toml"
```

## Traefik Dynamic Config
```
[http]

    # create router for http
    [http.routers.my-routers]
        entryPoints = ["http"]
        service = "service-http"
        rule = "PathPrefix(`/`)"

    # create router for api & dashboard
    [http.routers.my-api]
        entryPoints = ["api"]
        service = "api@internal"
        middlewares = ["auth"]
        rule = "(PathPrefix(`/api`) || PathPrefix(`/dashboard`))"

    # create middleware for enable httpasswd
    [http.middlewares]
        [http.middlewares.auth.basicAuth]
            users = [
                "admin:$apr1$dYf1urC5$pQca2NzLNmCFRGbCqWdiK."
            ]

    # create load balancer for service-http
    [http.services]
        [http.services.service-http.loadBalancer]
            [[http.services.service-http.loadBalancer.servers]]
                url = "http://localhost:8081"
            [[http.services.service-http.loadBalancer.servers]]
                url = "http://localhost:8082"
```

## Generate Password with htpasswd
`htpasswd -c .htpasswd admin`

change admin as user what you want.

### RUN
`sudo ./traefik --configfile=traefik.toml`
