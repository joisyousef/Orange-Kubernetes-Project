# syntax=docker/dockerfile:1.4
FROM --platform=$BUILDPLATFORM golang:1.18-alpine AS builder

# Set the working directory
WORKDIR /code

# Environment variables to disable CGO and set caching paths
ENV CGO_ENABLED 0
ENV GOPATH /go
ENV GOCACHE /go-build

# Copy go.mod and go.sum for dependency resolution
COPY go.mod go.sum ./
RUN --mount=type=cache,target=/go/pkg/mod/cache \
    go mod download

# Copy the rest of the application code
COPY . .

# Build the Go application
RUN --mount=type=cache,target=/go/pkg/mod/cache \
    --mount=type=cache,target=/go-build \
    go build -o bin/backend main.go

# Final stage to run the binary
FROM scratch
COPY --from=builder /code/bin/backend /usr/local/bin/backend

# Command to run the backend binary
CMD ["/usr/local/bin/backend"]
