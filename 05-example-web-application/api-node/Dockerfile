# Use the official Node.js image based on Alpine Linux for a lightweight base image
FROM node:alpine3.21

# Set the working directory inside the container
WORKDIR /usr/src/app

# Set the environment variable to production for optimized builds
ENV NODE_ENV=production

# Copy package.json and package-lock.json files to the working directory
COPY ./package*.json .

# Install production dependencies using npm with caching to speed up builds
# It used docker buildkit to cache. On windows, this feature is still under experiment.
# RUN --mount=type=cache,target=/usr/src/app/.npm \
#   && npm config set cache /usr/src/app/.npm \
#   && npm ci

# Without docker buildkit
RUN npm ci

# Switch to a non-root user for better security
USER node

# Copy the rest of the application code to the working directory with proper ownership
COPY --chown=node:node . .

# Expose port 3000 for the application
EXPOSE 3000

# Define the default command to run the application
CMD ["node", "src/index.js"]