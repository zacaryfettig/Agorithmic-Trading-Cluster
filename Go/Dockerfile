#build Stage
FROM golang:1.24-alpine AS build

# Install CA certs
RUN apk add --no-cache ca-certificates

WORKDIR /app

# Copy go mod files first (enables Docker layer caching)
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the source
COPY . .

# Build a statically linked binary
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app

# -------- Runtime stage --------
FROM alpine:3.20

# Install CA certs for HTTPS
RUN apk add --no-cache ca-certificates

WORKDIR /app

# Copy binary from buid
COPY --from=build /app/app .

# Run the app
CMD ["./app"]