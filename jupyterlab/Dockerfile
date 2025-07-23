# Start with mrdoge/ubuntu image
FROM mrdoge/ubuntu:latest
ENV Python_version=3.12.4

# Update environment
USER root
WORKDIR /root
RUN apt update && \
    apt upgrade -y && \
    apt autoremove -y

# Install IRkernel for R
RUN Rscript -e "install.packages('remotes')" && \
    Rscript -e "remotes::install_github(repo = 'IRkernel/IRkernel')"

# Switch to default user
USER ${default_user}
WORKDIR /home/${default_user}

# Install Miniconda
RUN mkdir -p /home/${default_user}/miniconda3 && \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /home/${default_user}/miniconda3/miniconda.sh && \
    bash /home/${default_user}/miniconda3/miniconda.sh -b -u -p /home/${default_user}/miniconda3 && \
    rm -rf /home/${default_user}/miniconda3/miniconda.sh && \
    /home/${default_user}/miniconda3/bin/conda init --all
ENV PATH=/home/${default_user}/miniconda3/bin:$PATH
RUN conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main && \
    conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r

# Initialize JupyterLab conda env
ENV CONDA_DEFAULT_ENV=JupyterLab
RUN conda create --name ${CONDA_DEFAULT_ENV} python=${Python_version} -y
ENV CONDA_PREFIX=/home/${default_user}/miniconda3/envs/${CONDA_DEFAULT_ENV}
ENV PATH=${CONDA_PREFIX}/bin:$PATH
RUN echo "" >> /home/${default_user}/.bashrc && \
    echo "conda activate ${CONDA_DEFAULT_ENV}" >> /home/${default_user}/.bashrc

# Install JupyterLab
RUN conda install -c conda-forge tini -y && \
    conda install -c conda-forge jupyterlab -y

# Add R kernel to JupyterLab
RUN Rscript -e "IRkernel::installspec(name = 'R',displayname = 'R')"

# Expose the JupyterLab web port
EXPOSE 8888

# Run JupyterLab
ENTRYPOINT ["tini","-g","--"]
CMD ["jupyter","lab","--ip=0.0.0.0","--port=8888","--no-browser"]
