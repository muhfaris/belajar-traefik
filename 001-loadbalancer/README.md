## Config
```
defaultEntryPoints = ["http"]

# enable dashboard
[api]
    dashboard = true
    insecure = true

# create entrypoint
[entryPoints]
    [entryPoints.http]
        address = ":8888"

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

    # create load balancer for service-http
    [http.services]
        [http.services.service-http.loadBalancer]
            [[http.services.service-http.loadBalancer.servers]]
                url = "http://localhost:8081"
            [[http.services.service-http.loadBalancer.servers]]
                url = "http://localhost:8082"
```

### RUN
`sudo ./traefik --configfile=traefik.toml`
