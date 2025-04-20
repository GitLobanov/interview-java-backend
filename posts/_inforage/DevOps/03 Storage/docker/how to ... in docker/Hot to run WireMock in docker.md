![](../../../../../_res/Pasted%20image%2020250117152605.png)

# mappings

```bash
{
    "request": {
      "url": "/api/test",
      "method": "GET"
    },
    "response": {
      "status": 200,
      "body": "{ \"test\": \"data\" }",
      "headers": {
        "Content-Type": "application/json"
      }
    }
}
```
# Docker compose

```bash
services:
  wiremock:
    ports:
      - 21040:8080
    image: "wiremock/wiremock:latest"
    container_name: wiremock
    volumes:
      - ./__files:/home/wiremock/__files
      - ./mappings:/home/wiremock/mappings
    entrypoint: ["/docker-entrypoint.sh", "--global-response-templating", "--disable-gzip", "--verbose"]
    networks:
      - concifora_default

networks:
  concifora_default:
    external: true
```