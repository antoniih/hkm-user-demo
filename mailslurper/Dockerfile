FROM golang:alpine as builder

RUN apk --no-cache add git libc-dev gcc
RUN go get github.com/mjibson/esc
WORKDIR /go/src/github.com/mailslurper
RUN git clone https://github.com/mailslurper/mailslurper.git
WORKDIR /go/src/github.com/mailslurper/mailslurper/cmd/mailslurper
RUN go get; go generate; go build
WORKDIR /go/src/github.com/mailslurper/mailslurper/cmd/createcredentials
RUN go get; go generate; go build


FROM alpine:3.15

RUN apk add --no-cache ca-certificates \
 && echo -e '{\n\
  "wwwAddress": "0.0.0.0",\n\
  "wwwPort": 8081,\n\
  "wwwPublicURL": "",\n\
  "serviceAddress": "0.0.0.0",\n\
  "servicePort": 8085,\n\
  "servicePublicURL": "",\n\
  "smtpAddress": "0.0.0.0",\n\
  "smtpPort": 2500,\n\
  "dbEngine": "SQLite",\n\
  "dbHost": "",\n\
  "dbPort": 0,\n\
  "dbDatabase": "./mailslurper.db",\n\
  "dbUserName": "",\n\
  "dbPassword": "",\n\
  "maxWorkers": 1000,\n\
  "autoStartBrowser": false,\n\
  "keyFile": "",\n\
  "certFile": "",\n\
  "adminKeyFile": "",\n\
  "adminCertFile": ""\n\
  }'\
  >> config.json

COPY --from=builder /go/src/github.com/mailslurper/mailslurper/cmd/mailslurper/mailslurper mailslurper
COPY --from=builder /go/src/github.com/mailslurper/mailslurper/cmd/createcredentials/createcredentials createcredentials
EXPOSE 8081 8085 2500
CMD ["./mailslurper"]