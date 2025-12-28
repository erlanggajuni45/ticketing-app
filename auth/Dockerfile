FROM golang:1.25.5-alpine AS base

WORKDIR /app
RUN apk add --no-cache git

COPY go.mod go.sum ./
RUN go mod download

FROM base AS dev
RUN go install github.com/air-verse/air@latest

COPY . .
CMD ["air", "-c", ".air.toml"]

FROM base AS builder
COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -o /main main.go

FROM alpine:3.19 AS production
RUN adduser -D appuser
USER appuser

WORKDIR /app

COPY --from=builder /main .

EXPOSE 3000

CMD ["./main"]