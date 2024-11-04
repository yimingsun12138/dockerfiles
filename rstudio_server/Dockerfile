# Start with mrdoge/ubuntu image
FROM mrdoge/ubuntu:latest
ENV Rstudio_server_url=https://s3.ap-east-1.amazonaws.com/mrdoge-s3-bucket/share/resource_for_download/rstudio_server_for_mrdoge_ubuntu.deb

# Update environment
USER root
WORKDIR /root
RUN apt update && \
    apt upgrade -y && \
    apt autoremove -y

# Install Rstudio server
RUN mkdir -p /tmp/Rstudio_server_install && \
    wget ${Rstudio_server_url} -O /tmp/Rstudio_server_install/rstudio_server.deb && \
    gdebi -n /tmp/Rstudio_server_install/rstudio_server.deb && \
    rm -rf /tmp/Rstudio_server_install/rstudio_server.deb

# Create sudo user rstudio to run Rstudio server
RUN useradd -m -s /bin/bash rstudio && \
    echo "rstudio ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Switch to user rstudio
USER rstudio
WORKDIR /home/rstudio

# Expose the Rstudio server web port
EXPOSE 8787

# Run Rstudio server
CMD ["sudo","/usr/lib/rstudio-server/bin/rserver","--server-daemonize=0","--www-port=8787"]
