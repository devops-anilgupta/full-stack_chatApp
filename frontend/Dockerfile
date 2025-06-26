# Build stage
FROM node:18-alpine as build

# Set working directory
WORKDIR /app

# Set a reliable npm registry to avoid DNS issues
RUN npm config set registry https://registry.npmjs.org/

# Copy only dependency files first (for better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application
COPY . .

# Build the app
RUN npm run build

# Production stage
FROM nginx:alpine

# Copy build output to Nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom Nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
