# Stage 1: Build NGINX from source
FROM alpine AS builder

RUN apk add --no-cache \
    build-base \
    pcre-dev \
    zlib-dev \
    openssl-dev

WORKDIR /tmp/nginx

# Download NGINX source code
RUN wget http://nginx.org/download/nginx-1.21.0.tar.gz && \
    tar -xzvf nginx-1.21.0.tar.gz

WORKDIR /tmp/nginx/nginx-1.21.0

# Configure NGINX build
RUN ./configure \
    --prefix=/usr/local/nginx \
    --with-http_ssl_module \
    --with-http_realip_module \
    --with-http_gzip_static_module \
    --with-http_stub_status_module \
    --with-pcre \
    --with-zlib \
    --with-openssl

# Build NGINX
RUN make && make install

# Stage 2: Create minimal runtime image
FROM alpine

# Copy NGINX binary and configuration from builder stage
COPY --from=builder /usr/local/nginx /usr/local/nginx

# Expose port 80
EXPOSE 80

# Command to start NGINX
CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]


