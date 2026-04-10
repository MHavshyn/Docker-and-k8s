# Build stage
FROM golang:1.23-alpine AS builder
WORKDIR /app

COPY simple-app/go.mod simple-app/go.sum ./
RUN go mod download

COPY  simple-app/. .
RUN go build -o main .

# Runtime stage
FROM alpine:latest
WORKDIR /app

# Create non-root user
RUN adduser -D appuser

# Copy only necessary files
COPY --from=builder /app/main ./main
COPY --from=builder /app/static ./static
COPY --from=builder /app/templates ./templates

USER appuser
EXPOSE 8080
ENTRYPOINT ["/app/main"]