FROM ubuntu:20.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update --fix-missing && \
  apt-get install -y wget bzip2 build-essential \
  less vim \
# snakemake:
  ca-certificates git libglib2.0-0 libxext6 libsm6 \
  libxrender1 git mercurial subversion python3-dev && \
  apt-get clean -y

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py37_4.8.3-Linux-x86_64.sh -O ~/miniconda.sh && \
  /bin/bash ~/miniconda.sh -b -p /opt/conda && \
  rm ~/miniconda.sh && \
  /opt/conda/bin/conda clean -tipsy && \
  ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
  echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
  echo "conda activate base" >> ~/.bashrc && \
  find /opt/conda/ -follow -type f -name '*.a' -delete && \
  find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
  /opt/conda/bin/conda clean -afy

ENV PATH /opt/conda/bin:$PATH
RUN conda update conda

# install mamba
RUN conda install -n base -c conda-forge mamba
RUN conda install -n base -c anaconda ipython
RUN mamba install -n base -c conda-forge -c bioconda \
  snakemake \
  hail

COPY /entrypoint.sh /opt/conda/bin/entrypoint.sh
RUN chmod a+x /opt/conda/bin/entrypoint.sh
CMD [".", "/opt/conda/bin/entrypoint.sh"]
