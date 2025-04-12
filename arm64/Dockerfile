FROM alpine:3.13.5

# Install required packages
RUN apk add --no-cache tini ttyd socat nginx unzip openssl openssh ca-certificates && \
    # Nginx configuration
    echo "daemon off;" >> /etc/nginx/nginx.conf && \
    mkdir /run/nginx && \
    echo "server { listen 80 default_server; root /var/www/localhost/htdocs; location / { try_files \$uri /index.html; } }" > /etc/nginx/conf.d/default.conf && \
    echo "<html><head><title>Welcome to Nginx</title></head><body><h1>Nginx works!</h1></body></html>" > /var/www/localhost/htdocs/index.html && \
    chown -R 0:82 /var/www/localhost/htdocs && \
    # Install ngrok
    wget -q https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-arm64.tgz && \
    tar -zxf ngrok-stable-linux-arm64.tgz && \
    mv ngrok /bin/ngrok && \
    rm ngrok-stable-linux-arm64.tgz && \
    echo "web_addr: 0.0.0.0:4040" > /root/ngrok.yml && \
    # Shell customization
    echo "export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> /etc/profile && \
    echo "alias poweroff='kill 1'" >> /etc/profile

# Set environment variables (optional default values)
ENV TINI_KILL_PROCESS_GROUP=1
ENV TTYD_USER=admin
ENV TTYD_PASS=admin

# Expose ports for ttyd, ngrok, and nginx
EXPOSE 7681 4040 80

# Entrypoint
ENTRYPOINT ["/sbin/tini", "--"]

# CMD uses env vars to set ttyd basic auth
CMD ["sh", "-c", "ttyd -s 3 -t titleFixed=/bin/sh -t rendererType=webgl -t disableLeaveAlert=true --credential ${TTYD_USER}:${TTYD_PASS} /bin/sh -i -l"]
