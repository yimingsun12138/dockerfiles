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

# Install dependency
RUN apt install -y sudo git vim wget curl python3 python3-venv python3-pip

# Create sudo user knight
RUN useradd -m -s /bin/bash ${default_user} && \
    echo "${default_user}:${default_password}" | chpasswd && \
    usermod -G sudo ${default_user}

# Install R
RUN apt install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common libcurl4-gnutls-dev libpango-1.0-0 libpango1.0-dev \
    openjdk-11-jdk libx11-dev libxext-dev libxtst-dev libxrender-dev libxmu-dev libxmuu-dev tcl tcl-dev tk tk-dev libtiff-dev libharfbuzz-dev \
    libfribidi-dev libgsl-dev libudunits2-dev libmagick++-dev libglpk-dev
RUN wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" && \
    apt install -y r-base && \
    add-apt-repository ppa:c2d4u.team/c2d4u4.0+

# Install other dependency
RUN apt install -y screen && \
    apt install -y expect && \
    apt install -y nfs-common

# Switch to user knight
USER ${default_user}
WORKDIR /home/${default_user}

# Run bash shell
CMD ["/bin/bash"]
