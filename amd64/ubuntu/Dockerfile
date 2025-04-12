FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Install packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    tini ttyd socat nginx unzip openssl openssh-client ca-certificates \
    wget screen curl && \
    # Clean up APT when done
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    # Configure nginx
    echo "daemon off;" >> /etc/nginx/nginx.conf && \
    mkdir -p /run/nginx && \
    echo "server { listen 80 default_server; root /var/www/html; location / { try_files \$uri /index.html; } }" > /etc/nginx/sites-available/default && \
    ln -sf /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default && \
    echo "<html><head><title>Welcome to Nginx</title></head><body><h1>Nginx works!</h1></body></html>" > /var/www/html/index.html && \
    # Install ngrok (amd64)
    wget -q https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip && \
    unzip ngrok-stable-linux-amd64.zip && \
    mv ngrok /usr/local/bin/ngrok && \
    rm ngrok-stable-linux-amd64.zip && \
    echo "web_addr: 0.0.0.0:4040" > /root/ngrok.yml && \
    # Customize shell
    echo "export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '" >> /etc/profile && \
    # Add fake poweroff alias
    echo "alias poweroff='kill 1'" >> /etc/profile

# Set environment variables for TINI and ttyd credentials
ENV TINI_KILL_PROCESS_GROUP=1
ENV TTYD_USER=admin
ENV TTYD_PASS=admin

# Expose necessary ports
EXPOSE 7681 4040 80

# Use Tini as entrypoint
ENTRYPOINT ["/usr/bin/tini", "--"]

# Default command
CMD ["sh", "-c", "ttyd -s 3 -t titleFixed=/bin/sh -t rendererType=webgl -t disableLeaveAlert=true --credential ${TTYD_USER}:${TTYD_PASS} /bin/bash -i -l"]
