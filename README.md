### [tutorial](https://blog.hasura.io/the-ultimate-guide-to-writing-dockerfiles-for-go-web-apps-336efad7012c)


- mounting host dir on docker image
```
docker build -t go-docker-dev .
docker run --rm -it -p 8080:8080 -v $(pwd):/go/src/app go-docker-dev bash
/go/src/app# go run src/main.go

```

- pkg management n layering using glide

glide.yaml and glide.lock are being added separately (instead of doing a ADD . .), resulting in separate layers. By separating out package management to a separate layer, Docker will cache the layer and will only rebuild it if the corresponding files change, i.e. when a new package is added or an existing one is removed. Hence, glide install won’t be executed for every source code change.

```
docker build -t go-docker-dev .
docker run --rm -it -p 8080:8080 -v $(pwd):/go/src/app go-docker-dev bash
/go/src/app# glide get github.com/golang/glog
```

Glide will install all packages into a vendor directory, which can be gitignore-d and dockerignore-d. It uses glide.lock to lock packages to specific versions. To (re-)install all packages mentioned in glide.yaml, execute:

```
docker run --rm -it -p 8080:8080 -v $(pwd):/go/src/app go-docker-dev bash
/go/src/app# glide install
```

- live reloading
```
docker build -t go-docker-dev .
docker run --rm -it -p 8080:8080 -v $(pwd):/go/src/app go-docker-dev bash
/go/src/app# gin --path src --port 8080 run main.go
```

All edits in src directory will trigger a rebuild and changes will be available live at http://localhost:8080.

