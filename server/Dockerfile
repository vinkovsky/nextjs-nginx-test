FROM node:16

# Installing libvips-dev for sharp compatability
RUN apt-get update && apt-get install -y libvips-dev

# Set working directory
WORKDIR /usr/app

# Install PM2 globally
RUN yarn global add pm2

# Copy "package.json" and "package-lock.json" before other files
# Utilise Docker cache to save re-installing dependencies if unchanged
COPY ./package.json ./
COPY ./yarn.lock ./

# Install dependencies
RUN yarn install

# Copy all files
COPY ./ ./

# Build app
RUN yarn build

# Expose the listening port
EXPOSE 1337

# Run container as non-root (unprivileged) user
# The "node" user is provided in the Node.js Alpine base image
USER node

# Launch app with PM2
CMD ["pm2-runtime", "start", "yarn", "--interpreter", "sh", "--name", "web-interface", "--", "start"]
