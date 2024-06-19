# dockerfiles
Various Dockerfiles I use on servers.

## Motivation
I am a loyal user of [Google Colab](https://colab.research.google.com/). With the help of my self-written Python package, [ColabGeek](https://github.com/yimingsun12138/ColabGeek), I can easily create various services and configure personalized IDEs and data analysis environments on Colab. However, when it comes to the Jupyter IDE, configuring a personalized environment on Colab is extremely difficult since Colab itself is a Jupyter server. My solution to this problem is to use [udocker](https://github.com/indigo-dc/udocker) to run a Docker container on Colab, completely resolving the conflict between the Colab environment and the Jupyter environment that I want to create. Therefore, in this GitHub repository, I will collect and organize the Dockerfiles I have created. The Docker images generated from these Dockerfiles can be found in my [DockerHub repository](https://hub.docker.com/u/mrdoge).

## Images

### ubuntu
I prefer using ubuntu as my Linux operating system. Therefore, I've tailored the [official ubuntu image](https://hub.docker.com/_/ubuntu) according to my needs. This includes installing several dependencies I regularly use in my data analysis work. Additionally, I've created a sudo user with the username 'knight' and the password 'midnight'. Moving forward, this customized ubuntu image will serve as the foundation for the majority of the Docker images I create.

docker usage:
```
docker pull mrdoge/ubuntu
docker run -it mrdoge/ubuntu
```

udocker usage:
```
udocker pull mrdoge/ubuntu
udocker run --user knight mrdoge/ubuntu /bin/bash
```

### jupyterlab
A [jupyterlab](https://jupyterlab.readthedocs.io/en/latest/index.html) image built on my personalized ubuntu image, featuring Python version 3.12.4, R version 4.4.1. The token for this jupyterlab server will be generated randomly. Please check the console after executing the `docker run` command for the token.

docker usage:
```
# To get the jupyterlab token, run this image interactively
docker pull mrdoge/jupyterlab
docker run -it -p 8787:8888 mrdoge/jupyterlab

# To detach the session, use the screen command
screen -d -m docker run -it -p 8787:8888 mrdoge/jupyterlab

# You can also run this image in background and get the token using docker logs
docker run -d -p 8787:8888 mrdoge/jupyterlab
docker ps # get your container id
docker logs your-container-id
```

udocker usage:
```
udocker pull mrdoge/jupyterlab
screen -d -m udocker run -p 8787:8888 mrdoge/jupyterlab
```
