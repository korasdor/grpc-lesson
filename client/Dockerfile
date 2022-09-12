# builder image
FROM golang:1.19-alpine AS build-env

ENV APP_NAME grpc-client

RUN apk --no-cache add build-base git breezy mercurial gcc

COPY client /go/src/
COPY pkg /go/src/pkg/
COPY go.mod go.sum /go/src/
WORKDIR /go/src/

RUN go mod download
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o ${APP_NAME}
RUN chmod +x ${APP_NAME}

FROM alpine

ENV APP_NAME grpc-client

WORKDIR /app
COPY --from=build-env /go/src/${APP_NAME} /app/
COPY --from=build-env /go/src/.env /app/

ENTRYPOINT ./${APP_NAME}