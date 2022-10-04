FROM debian:stretch-slim@sha256:d27bbe1259aeb6bed459440649ce5bda9083ab9782274c7bc469f02f283a9e18

#Install necessary libraries, matplotlib for python, and samtools
RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    wget \
    build-essential \
    bzip2 \
    git \
### Repair FASTQ:
    make \
    gcc \
    g++ \
    cmake \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

### Install Miniconda:
ENV CONDA_DIR /opt/conda
#RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
COPY miniconda.sh /tmp
RUN /bin/bash /tmp/miniconda.sh -b -p /opt/conda
# Put conda in path so we can use conda activate:
ENV PATH=$CONDA_DIR/bin:$PATH

### Snakemake:
RUN conda update -n base -c defaults -y conda
RUN conda install -n base -c conda-forge -y mamba
RUN mamba install -c conda-forge -c bioconda -y snakemake-minimal
RUN conda clean --all -y

### FASTQ Repair:
COPY fastq_pair_v1.0.tar.gz /tmp

RUN cd /tmp \
    && tar -xf fastq_pair_v1.0.tar.gz \
    && cd fastq-pair-1.0/ \
    && mkdir build && cd build \
    && cmake ../ \
    && make && make install \
    && rm /tmp/fastq_pair_v1.0.tar.gz \
    && rm -rf /tmp/fastq-pair-1.0
