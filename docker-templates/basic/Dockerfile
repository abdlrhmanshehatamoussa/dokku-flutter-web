# Use lightweight nginx
FROM nginx:alpine

# Remove default nginx site config
RUN rm /etc/nginx/conf.d/default.conf
RUN rm -rf /usr/share/nginx/html/*

# Add custom nginx configuration directly from Dockerfile
RUN printf '%s\n' \
  'server {' \
  '  listen 80;' \
  '  server_name _;' \
  '' \
  '  root /usr/share/nginx/html;' \
  '  index index.html;' \
  '' \
  '  # Allow only direct access to /index.html' \
  '  location = /index.html {' \
  '    try_files $uri =404;' \
  '  }' \
  '' \
  '  # Prevent direct access to any file except index.html' \
  '  location ~* ^/(?!index\.html$).+$ {' \
  '    return 404;' \
  '  }' \
  '' \
  '  # All other requests (SPA support): always serve index.html' \
  '  location / {' \
  '    try_files /index.html =404;' \
  '  }' \
  '}' > /etc/nginx/conf.d/default.conf

# Copy Flutter web build
COPY build/web/ /usr/share/nginx/html

# Expose port (Dokku will map it automatically)
EXPOSE 80

# Run nginx in foreground
CMD ["nginx", "-g", "daemon off;"]
