# Get image
FROM node:19.6-alpine

# Set working directory for the application inside the container
WORKDIR /usr/src/app

# Set environment variable to production
ENV NODE_ENV=production

# Copy the package.json and package-lock.json files to the container
COPY package.json package-lock.json ./

# Run npm ci to install dependencies based on package-lock.json
# --only=production ensures that only production dependencies are installed, not devDependencies
# Using a cache mount to cache npm modules between builds for faster installs
# Note: This requires BuildKit to be enabled. You can enable it by setting the environment variable DOCKER_BUILDKIT=1
# Example: export DOCKER_BUILDKIT=1
# Then build the image using: docker build -t your-image-name .
RUN --mount=type=cache,target=/usr/src/app/.npm \
npm set cache /usr/src/app/.npm && \
npm ci --only=production

# Create a non root user 
USER node 

# Copy ./src and set ownership to the non root user
COPY --chown=node:node ./src .

# Execute on runtime
CMD ["node", "index.js"]
