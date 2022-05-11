FROM gcr.io/ris-registry-shared/parabricks:latest

#Copy local plotReads python program
#COPY bamMetrics_modified.cpp /tmp
#COPY setup.sh /opt

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ARG DEBIAN_FRONTEND=noninteractive

# Due to some error...
# W: GPG error: https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64  InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY A4B469963BF863CC
# E: The repository 'https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64  InRelease' is no longer signed.
# --> Github Issue: https://github.com/NVIDIA/nvidia-docker/issues/1632#issuecomment-1112667716
RUN rm /etc/apt/sources.list.d/cuda.list

RUN apt-get update --fix-missing && \
  apt-get install -y wget bzip2 build-essential \
  less vim git \
# bamMetrics:
#  make gcc g++ lbzip2 zlib1g-dev \
#  libbz2-dev liblzma-dev libcurl4-gnutls-dev \
#  libssl-dev libncurses5-dev perl \
  && apt-get clean -y

#RUN cd /tmp \
#    && g++ -O2 -o /usr/bin/bamMetrics /tmp/bamMetrics_modified.cpp 

### Already had miniconda v4.12.0
RUN conda install -n base -c conda-forge mamba -y
RUN mamba install -n base -c conda-forge -c bioconda snakemake -y
RUN conda clean --all -y
