FROM ubuntu:20.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y --fix-missing \
    && apt-get install -y --no-install-recommends \
    wget \
    default-jdk \
    make \
    gcc \
    g++ \
    ca-certificates \
    lbzip2 \
    zlib1g-dev \
    libbz2-dev \
    liblzma-dev \
    libcurl4-gnutls-dev \
    libssl-dev \
    libncurses5-dev \
    perl git \
    less vim rsync && \
    apt-get clean -y

# Miniconda v4.9 (Py38)
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py38_4.9.2-Linux-x86_64.sh -O ~/miniconda.sh && \
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

# Nextflow v21 --> v19.01.0
RUN conda install -n base -c conda-forge -c bioconda nextflow
#RUN wget -qO- https://get.nextflow.io | bash && \
#  chmod +x nextflow && mv nextflow /usr/local/bin

# R v4.0.1 --> v4.1.0
RUN conda install -n base -c conda-forge r-base=4.1.0

# VirMutSig:
RUN cd /opt && git clone https://github.com/BIMIB-DISCo/VirMutSig.git

# Configured sra toolkit:
COPY ncbi_user-settings.mkfg /root
RUN mkdir -p ~/.ncbi && mv ~/ncbi_user-settings.mkfg ~/.ncbi/user-settings.mkfg

##### DeNovo:
# data.table v1.14.0
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/data.table/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('data.table');"

# ggplot2 v3.3.1 --> v3.3.5 (tidyverse v1.3.0)
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/tidyverse/index.html
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/ggplot2/index.html
RUN R -e "options(repos = list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
	install.packages('tidyverse'); install.packages('ggplot2');"

# gridExtra v2.3
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/gridExtra/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('gridExtra');"

# NMF v0.22.0 --> 0.23.0 (BiocManager v1.30.16)
# https://bioconductor.org/packages/release/bioc/html/Biobase.html
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/BiocManager/index.html
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/NMF/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
	install.packages('BiocManager'); \
	BiocManager::install('Biobase'); \
        install.packages('NMF');"

# nnls v0.21.0(x) --> v1.4???
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/nnls/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('nnls');"

# optparse v1.6.2 --> v1.6.6
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/optparse/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('optparse');"

# parallel v3.6.3

# stringi v1.6.2 --> 1.7.3
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/stringi/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('stringi');"

# yaml v2.2.0 --> 2.2.1
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/yaml/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('yaml');"

#### Assignment:
# glmnet v4.1_1 --> v4.1_2
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/glmnet/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('glmnet');"

# lsa 0.73.2
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/lsa/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('lsa');"

# Matrix 1.2_17 --> v1.3_4
# https://cran.microsoft.com/snapshot/2021-07-26/web/packages/Matrix/index.html
RUN R -e "options(repos = \
        list(CRAN = 'https://cran.microsoft.com/snapshot/2021-07-26')); \
        install.packages('Matrix');"

COPY /entrypoint.sh /opt/conda/bin/entrypoint.sh
RUN chmod a+x /opt/conda/bin/entrypoint.sh
CMD [".", "/opt/conda/bin/entrypoint.sh"]
