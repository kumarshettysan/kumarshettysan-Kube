# Dockerfile
FROM golang:1.17 AS builder

WORKDIR /go/src/app
COPY . .

RUN go build -o device-plugin .

FROM debian:bullseye-slim

WORKDIR /app
COPY --from=builder /go/src/app/device-plugin .

CMD ["./device-plugin"]
