### [tutorial](https://blog.hasura.io/the-ultimate-guide-to-writing-dockerfiles-for-go-web-apps-336efad7012c)


```

$ docker build -t go-docker-dev .
$ docker run --rm -it -p 8080:8080 -v $(pwd):/go/src/app go-docker-dev bash
/go/src/app# go run src/main.go

```