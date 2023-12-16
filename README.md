# dockerfiles
Various Dockerfiles I use on servers.

## Motivation
I am a loyal user of Google Colab. With the help of my self-written Python package, [ColabGeek](https://github.com/yimingsun12138/ColabGeek), I can easily create various services and configure personalized IDEs and data analysis environments on Colab. However, when it comes to the Jupyter IDE, configuring a personalized environment on Colab is extremely difficult since Colab itself is a Jupyter server. My solution to this problem is to use [udocker](https://github.com/indigo-dc/udocker) to run a Docker container on Colab, completely resolving the conflict between the Colab environment and the Jupyter environment that I want to create. Therefore, in this GitHub repository, I will collect and organize the Dockerfiles I have created. The Docker images generated from these Dockerfiles can be found in my [DockerHub repository](https://hub.docker.com/u/mrdoge).

## Images

### ubuntu
I prefer using Ubuntu as my Linux operating system. Therefore, I've tailored the [official ubuntu image](https://hub.docker.com/_/ubuntu) according to my needs. This includes installing several dependencies I regularly use in my data analysis work. Additionally, I've created a sudo user with the username 'knight' and the password 'midnight'. Moving forward, this customized Ubuntu image will serve as the foundation for the majority of the Docker images I create.

### jupyterlab
A JupyterLab image built on my personalized Ubuntu image, featuring Python version 3.10.12. The token for this JupyterLab server will be generated randomly. Please check the console after executing the docker run command for the token.
