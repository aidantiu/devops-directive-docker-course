# Build stage
FROM golang:1.19-buster as build

# Set working directory for the application inside the container
WORKDIR /app

# Set user to non-root user
RUN useradd -u 1001 nonroot


# Copy go.mod and go.sum files to the container
COPY go.mod go.sum ./

# Copy source code to working directory
COPY . .

# Download Go modules and cache mount them
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    go mod download

# Build the Go application
RUN go build \
    -ldflags="linkmode=external -extldflags=-static" \
    -tags netgo \
    -o api-golang



### Multi-stage build to create a smaller final image
FROM scratch

# Set GIN_MODE to release for production
ENV GIN_MODE release

# Copy /etc/passwd from the golang image to have the non-root user in the final image
COPY --from=build /etc/passwd /etc/passwd   

# Switch to non-root user
USER nonroot

# Copy the built binary from the build stage
COPY --from=build /app/api-golang api-golang

# Expose port 8080 to the outside world
EXPOSE 8080

# Run the built executable
CMD ["./api-golang"]