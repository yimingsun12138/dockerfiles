# Start with ubuntu base image
FROM ubuntu:latest
ENV DEBIAN_FRONTEND=noninteractive
ENV SHELL=/bin/bash
ENV default_user=knight
ENV default_password=midnight

# Update environment
USER root
WORKDIR /root
RUN apt update && \
    apt upgrade -y && \
    apt autoremove -y

# Install dependencies
RUN apt install -y sudo openssh-server rsync git vim wget curl \
    python3 python3-venv python3-pip \
    gdebi-core screen expect nfs-common

# Create default user
RUN useradd -m -s /bin/bash ${default_user} && \
    echo "${default_user}:${default_password}" | chpasswd && \
    usermod -G sudo ${default_user}

# Configure OpenSSH
RUN mkdir -p /run/sshd && \
    sed -i 's/#Port 22/Port 22/' /etc/ssh/sshd_config && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/KbdInteractiveAuthentication no/KbdInteractiveAuthentication yes/' /etc/ssh/sshd_config

# Install R
RUN apt install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common \
    libcurl4-gnutls-dev libpango-1.0-0 libpango1.0-dev openjdk-11-jdk libx11-dev libxext-dev libxtst-dev \
    libxrender-dev libxmu-dev libxmuu-dev tcl tcl-dev tk tk-dev libtiff-dev libharfbuzz-dev libfribidi-dev \
    libgsl-dev libudunits2-dev libmagick++-dev libglpk-dev libssl-dev libhdf5-dev cmake libgdal-dev
RUN wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" && \
    apt install -y r-base

# Switch to default user
USER ${default_user}
WORKDIR /home/${default_user}

# Expose the OpenSSH port
EXPOSE 22

# Run bash shell
CMD ["/bin/bash"]
