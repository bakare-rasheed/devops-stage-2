# Stage 1: Build the React app
FROM node:14 AS build

WORKDIR /app

# Copy the package.json and package-lock.json files to the container
COPY package*.json ./

# Clear any npm proxy settings
RUN npm config delete proxy
RUN npm config delete https-proxy

# Increase the network timeout for npm
RUN npm config set fetch-retry-mintimeout 20000
RUN npm config set fetch-retry-maxtimeout 120000

# Force clean install
RUN rm -rf node_modules package-lock.json
RUN npm cache clean --force

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the React app
RUN npm run build

# Stage 2: Serve the React app using Nginx
FROM nginx:alpine

# Copy the built React app from the previous stage
COPY --from=build /app/dist /usr/share/nginx/html

# Copy the Nginx configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
