# dockerfiles

Various Dockerfiles I use on servers.

Update: 2025/02/11

## Motivation

I am a loyal user of [Google Colab](https://colab.research.google.com/). With the help of my self-written Python package [ColabGeek](https://github.com/yimingsun12138/ColabGeek), I can easily run various services on Colab, including web IDEs like Rstudio server and code server. However, when it comes to the Jupyter IDE, configuring a personalized environment on Colab is extremely difficult since Colab itself is a Jupyter server. My solution to this problem is to use [udocker](https://github.com/indigo-dc/udocker) to run a Docker container on Colab, completely resolving the conflict between the Colab environment and the Jupyter environment that I want to create. Therefore, in this GitHub repository, I will collect and organize various Dockerfiles I use on servers. The Docker images generated from these Dockerfiles can be found in my [DockerHub repository](https://hub.docker.com/u/mrdoge).

## Images

### ubuntu

I prefer using Ubuntu as my Linux operating system. Therefore, I've tailored the [official ubuntu image](https://hub.docker.com/_/ubuntu) according to my own needs. This includes creating a sudo user "knight" with the password "midnight" and installing several dependencies regularly used in my data analysis workflow. Moving forward, this customized ubuntu image will serve as the foundation for other Docker images I create.

docker usage:

```
docker pull mrdoge/ubuntu
docker run -it mrdoge/ubuntu

# OpenSSH server is also supported by this ubuntu image
docker run -d --user root mrdoge/ubuntu /usr/sbin/sshd -D
```

udocker usage:

```
udocker pull mrdoge/ubuntu
udocker run --user knight mrdoge/ubuntu /bin/bash
```

### jupyterlab

A [jupyterlab](https://jupyterlab.readthedocs.io/en/latest/index.html) image built on my personalized ubuntu image, featuring Python version 3.12.4, R version 4.4.2. The login token for this JupyterLab server will be generated randomly. Please check the console after executing the `docker run` command for the login token.

docker usage:

```
# To get the JupyterLab login token, run this image interactively
docker pull mrdoge/jupyterlab
docker run -it -p 8888:8888 mrdoge/jupyterlab

# To detach the session, use the screen command
screen -d -m docker run -it -p 8888:8888 mrdoge/jupyterlab

# You can also run this image in background and get the login token using docker logs
docker run -d -p 8888:8888 mrdoge/jupyterlab
docker ps # get your container id
docker logs your-container-id
```

udocker usage:

```
udocker pull mrdoge/jupyterlab
screen -d -m udocker run -p 8888:8888 mrdoge/jupyterlab
```

### rstudio_server

A [rstudio server](https://posit.co/products/open-source/rstudio-server/) image built on my personalized ubuntu image. Note that the current image does not support running through udocker (sudo command is not allowed in udocker).

docker usage:

```
docker pull mrdoge/rstudio_server
docker run -d -p 8787:8787 mrdoge/rstudio_server
```

Sign in the Rstudio server web IDE using the default user "knight" mentioned above.
