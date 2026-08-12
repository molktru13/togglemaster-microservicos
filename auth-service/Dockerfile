# ---- Build Stage ----
FROM golang:1.21-alpine AS builder

WORKDIR /app

# Copia os módulos e baixa as dependências (cache em camadas)
COPY go.mod go.sum ./
RUN go mod download

# Copia o código-fonte e compila o binário
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/auth-service .

# ---- Runtime Stage ----
FROM alpine:3.19

RUN apk add --no-cache ca-certificates

WORKDIR /app
COPY --from=builder /app/auth-service /app/auth-service

EXPOSE 8001

CMD ["/app/auth-service"]
