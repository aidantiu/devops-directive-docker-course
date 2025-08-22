# Get image name
FROM node:19.6-alpine

# Working Directory
WORKDIR /usr/src/app

# Set environment variables
ENV NODE_ENV production

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install the application dependencies
RUN --mount=type=cache,target=/usr/src/app/.npm \
    npm set cache /usr/src/app/.npm && \
    npm ci --only=production

# Create user
USER node

# Copy the rest of the application code
COPY --chown=node:node ./src .

# Set port
EXPOSE 3000

# Execute in runtime env
CMD ["node", "index.js"]



