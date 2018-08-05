##

```
docker container run --name registry-app -e JHIPSTER.SECURITY.AUTHENTICATION.JWT.SECRET=dkk20dldkf0209342334 -d -p 8761:8761 jhipster/jhipster-registry:v4.0.0

docker container run --name jhipster -v /opt/jhipster:/home/jhipster/app -v /opt/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster:v5.1.0

docker exec -it jhipster bash
```