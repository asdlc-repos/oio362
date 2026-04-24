# syntax=docker/dockerfile:1

FROM golang:1.21-alpine AS builder
WORKDIR /src
COPY go.mod ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /out/hello-api .

FROM alpine:latest
RUN apk --no-cache add ca-certificates && \
    addgroup -S app && adduser -S app -G app
WORKDIR /app
COPY --from=builder /out/hello-api /app/hello-api
USER app
EXPOSE 9090
CMD ["/app/hello-api"]
