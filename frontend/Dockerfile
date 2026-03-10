# ----------------------------
# Stage 1: Build Flutter Web
# ----------------------------
FROM ghcr.io/cirruslabs/flutter:3.41.1 AS build

WORKDIR /app

# Copy project files into container
COPY . .

# Enable web support
RUN flutter config --enable-web

# Get dependencies
RUN flutter pub get

# Build web in release mode with API base URL pointing to EC2 public IP
RUN flutter build web --release --dart-define=API_BASE_URL=http://44.220.47.8:8090

# ----------------------------
# Stage 2: Serve with Nginx
# ----------------------------
FROM nginx:alpine

# Copy built web files into Nginx
COPY --from=build /app/build/web /usr/share/nginx/html

# Expose port 80 for frontend
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]