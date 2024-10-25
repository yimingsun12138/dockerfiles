# Start with mrdoge/ubuntu image
FROM mrdoge/ubuntu:latest
ENV rstudio_server_url=https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2024.09.0-375-amd64.deb

# Update environment
USER root
WORKDIR /root
RUN apt update && \
    apt upgrade -y && \
    apt autoremove -y

# Install rstudio server
RUN mkdir -p /tmp/rstudio_server_install && \
    wget ${rstudio_server_url} -O /tmp/rstudio_server_install/rstudio_server.deb && \
    gdebi -n /tmp/rstudio_server_install/rstudio_server.deb

# Expose the rstudio server web port
EXPOSE 8787

# Run rstudio server
CMD ["/usr/lib/rstudio-server/bin/rserver","--server-daemonize=0","--www-port=8787"]
