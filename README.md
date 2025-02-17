# API Firewall

Light-weighted API Firewall to protect your API endpoints in cloud-native environments with API Schema validation. API Firewall relies on a positive security model allowing calls that match predefined API specs, while rejecting everything else. 

Technically, API Firewall is a reverse proxy with a built-in OpenAPI v3 request and response validator, written in Go, and optimized for extreme performance and near-zero added latency.

## Installation

Docker deployment:

```
$ docker pull wallarm:api-firewall
$ docker run -v /opt/sample-api/openapi3/:/tmp -e SERVER_URL=http://172.17.0.1:9090/v1.0/ -e SWAGGER_FILE=/tmp/pets-api.yaml api-firewall
```

Kubernetes deployment:

```
TBD
```

## Configuration

### Environment options

#### Proxy section

```dockerfile
APIFW_PROXY_URL: "http://0.0.0.0:8081"
APIFW_PROXY_LOG_LEVEL: "DEBUG"  # could be INFO, ERROR, WARNING or DEBUG
APIFW_PROXY_LOG_FILE: "api-firewall.log"
APIFW_PROXY_READ_TIMEOUT: "5s"
APIFW_PROXY_WRITE_TIMEOUT: "5s"
APIFW_PROXY_SHUTDOWN_TIMEOUT: "5s"
```

#### TLS section

```dockerfile
APIFW_TLS_CERTS_PATH: "/api-firewall/resources/certs"
APIFW_TLS_CERT_FILE: "localhost.crt"
APIFW_TLS_CERT_KEY: "localhost.key"
```

#### Swagger section

File with API specs may have .json or .yaml extension and should be defined using OpeanAPI v3 standard.

```dockerfile
APIFW_SWAGGER_FILE: "/api-firewall/resources/swagger.json"
```

#### Server section

```dockerfile
APIFW_SERVER_URL: "http://example.com/v1/"
APIFW_SERVER_MAX_CONNS_PER_HOST: 512
APIFW_SERVER_READ_TIMEOUT: "5s"
APIFW_SERVER_WRITE_TIMEOUT: "5s"
APIFW_SERVER_DIAL_TIMEOUT: "200ms"
```


## Performance benchmarks

API Firewall relies on optimized HTTP library, sockets, and JSON assertions to deliver the fastest processing time. 

```
$ ab -n100000 -c 10 -p pet.json http://172.17.0.2:8282/v1.0/pets/1
This is ApacheBench, Version 2.3 <$Revision: 1879490 $>

Document Path:          /v1.0/pets/1
Document Length:        18 bytes

Concurrency Level:      10
Time taken for tests:   4.504 seconds
Complete requests:      100000
Failed requests:        0
Non-2xx responses:      100000
Total transferred:      22200000 bytes
Total body sent:        20500000
HTML transferred:       1800000 bytes
Requests per second:    22204.12 [#/sec] (mean)
Time per request:       0.450 [ms] (mean)
Time per request:       0.045 [ms] (mean, across all concurrent requests)
Transfer rate:          4813.78 [Kbytes/sec] received
                        4445.16 kb/s sent
                        9258.94 kb/s total
```

