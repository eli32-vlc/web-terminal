FROM ubuntu:20.04

# Install required packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    tini socat nginx unzip openssl openssh-client ca-certificates \
    wget screen curl && \
    # Download ttyd executable directly and install it
    wget -O ttyd https://github.com/tsl0922/ttyd/releases/download/1.6.0/ttyd_linux.x86_64 && \
    chmod +x ttyd && \
    mv ttyd /usr/local/bin/ && \
    # Configure nginx
    echo "daemon off;" >> /etc/nginx/nginx.conf && \
    mkdir -p /run/nginx && \
    echo "server { listen 80 default_server; root /var/www/html; location / { try_files \$uri /index.html; } }" > /etc/nginx/sites-available/default && \
    ln -sf /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default && \
    echo "<html><head><title>Welcome to Nginx</title></head><body><h1>Nginx works!</h1></body></html>" > /var/www/html/index.html && \
    # Customize shell and add fake poweroff alias
    echo "export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> /etc/profile && \
    echo "alias poweroff='kill 1'" >> /etc/profile

# Set environment variables for TINI and ttyd credentials
ENV TINI_KILL_PROCESS_GROUP=1
ENV TTYD_USER=admin
ENV TTYD_PASS=admin

# Expose necessary ports
EXPOSE 7681 80

# Entrypoint and CMD to start ttyd with bash shell
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["sh", "-c", "ttyd -s 3 -t titleFixed=/bin/sh -t rendererType=webgl -t disableLeaveAlert=true --credential ${TTYD_USER}:${TTYD_PASS} /bin/bash -i -l"]
