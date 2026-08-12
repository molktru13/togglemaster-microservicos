# ---- Build Stage ----
FROM golang:1.21-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/evaluation-service .

# ---- Runtime Stage ----
FROM alpine:3.19

RUN apk add --no-cache ca-certificates

WORKDIR /app
COPY --from=builder /app/evaluation-service /app/evaluation-service

EXPOSE 8004

CMD ["/app/evaluation-service"]
