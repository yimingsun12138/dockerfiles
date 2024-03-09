# Start with mrdoge/ubuntu image
FROM mrdoge/ubuntu:latest
ENV python_version=3.10.13
ENV r_version=4.3.3

# Update environment
USER root
WORKDIR /root
RUN apt update && \
    apt upgrade -y && \
    apt autoremove -y

# Switch to user knight
USER ${default_user}
WORKDIR /home/${default_user}

# Install miniconda
RUN mkdir -p /home/${default_user}/miniconda3 && \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /home/${default_user}/miniconda3/miniconda.sh && \
    bash /home/${default_user}/miniconda3/miniconda.sh -b -u -p /home/${default_user}/miniconda3 && \
    rm -rf /home/${default_user}/miniconda3/miniconda.sh && \
    /home/${default_user}/miniconda3/bin/conda init bash && \
    /home/${default_user}/miniconda3/bin/conda init zsh
ENV PATH=/home/${default_user}/miniconda3/bin:$PATH

# Init jupyterlab conda env
RUN conda install -c conda-forge conda-build -y
ENV CONDA_DEFAULT_ENV=jupyterlab
RUN conda create --name ${CONDA_DEFAULT_ENV} python=${python_version} -y
ENV CONDA_PREFIX=/home/${default_user}/miniconda3/envs/${CONDA_DEFAULT_ENV}
ENV PATH=${CONDA_PREFIX}/bin:$PATH
RUN echo "conda activate ${CONDA_DEFAULT_ENV}" >> /home/${default_user}/.bashrc

# Install jupyterlab
RUN conda install -c conda-forge tini -y && \
    conda install -c conda-forge r-base=${r_version} -y && \
    conda install -c conda-forge r-irkernel -y && \
    conda install -c conda-forge jupyterlab -y

# Expose the jupyterlab web port
EXPOSE 8888

# Run jupyterlab
ENTRYPOINT ["tini","-g","--"]
CMD ["jupyter","lab","--ip=0.0.0.0","--port=8888","--no-browser"]
