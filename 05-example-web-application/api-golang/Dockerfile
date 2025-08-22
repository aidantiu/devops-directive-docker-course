# Get golang image (use golang 1.19 based on Debian Buster)
FROM golang:1.19-buster as build

# Set working directory for the application inside the container
WORKDIR /app

# Create a non-root user with user ID 1001 for security purposes
RUN useradd -u 1001 nonroot

# Copy the go module files (go.mod and go.sum) to the container
COPY go.mod go.sum ./

# Install Go dependencies:
# - The first mount (`/go/pkg/mod`) caches Go modules, so they don't need to be re-downloaded in future builds
# - The second mount (`/root/.cache/go-build`) caches the build output to speed up subsequent builds
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    go mod download

# Copy the rest of the application code to the container
COPY . .

# Compile the Go application:
# - `-ldflags="-linkmode external -extldflags -static"` makes the binary statically linked
# - `-tags netgo` forces Go to use the `netgo` DNS resolver for better compatibility
# - `-o api-golang` specifies the output binary name
RUN go build \
    -ldflags="-linkmode external -extldflags -static" \
    -tags netgo \
    -o api-golang

### RELEASE STAGE

# Start a new minimal image based on 'scratch', which is an empty image with no extra libraries or files
FROM scratch

# Set the environment variable for the application (e.g., production mode for Gin framework)
ENV GIN_MODE release

# Copy the /etc/passwd file from the build stage to allow the 'nonroot' user to be recognized in the final image
COPY --from=build /etc/passwd /etc/passwd

# Copy the compiled binary from the build stage to the final image
COPY --from=build /app/api-golang api-golang

# Set the user to 'nonroot' for running the app (for security, it's not recommended to run as root)
USER nonroot

# Expose port 8080 (the port the Go application listens on)
EXPOSE 8080

# Set the command that will run when the container starts (runs the compiled Go binary)
CMD ["/api-golang"]
