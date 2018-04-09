FROM golang:1.10

# TODO: install a specific commit/version

# Client compat
RUN go get github.com/twitchtv/twirp/clientcompat

# Client example
RUN set -xe \
    && go get github.com/twitchtv/twirp/example/cmd/client \
    && sed -i -e 's|"http://localhost:8080"|os.Args[1]|g' /go/src/github.com/twitchtv/twirp/example/cmd/client/main.go \
    && sed -i -e 's|"net/http"|"net/http"\n"os"|g' /go/src/github.com/twitchtv/twirp/example/cmd/client/main.go \
    && go install github.com/twitchtv/twirp/example/cmd/client

# Server example
RUN go get github.com/twitchtv/twirp/example/cmd/server
