FROM ubuntu:20.04

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update --fix-missing && \
    apt install -y mlocate default-jdk \
    wget less vim \
# crcmod:
    gcc python3-dev python3-setuptools \
# gsuit:
    apt-transport-https gnupg curl && \
    apt-get clean -y

# gsuit:
# https://cloud.google.com/storage/docs/gsutil_install#install
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg  add - && apt-get update -y && apt-get install google-cloud-sdk -y

#RUN cd /opt/ && \
#    wget https://mirrors.gigenet.com/apache/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz && \
#    tar zxf spark-3.1.2-bin-hadoop3.2.tgz && \
#    rm spark-3.1.2-bin-hadoop3.2.tgz
#ENV SPARK_HOME /opt/spark-3.1.2-bin-hadoop3.2
#ENV PATH /opt/conda/bin/:$SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py37_4.10.3-Linux-x86_64.sh -O ~/miniconda.sh && \
  /bin/bash ~/miniconda.sh -b -p /opt/conda && \
  rm ~/miniconda.sh && \
  /opt/conda/bin/conda clean -tipsy && \
  ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
  echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
  echo "conda activate base" >> ~/.bashrc && \
  find /opt/conda/ -follow -type f -name '*.a' -delete && \
  find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
  /opt/conda/bin/conda clean -afy 

ENV PATH /opt/conda/bin/:$PATH
RUN conda update conda

# install mamba
RUN conda install -n base -c conda-forge mamba
RUN conda install -n base -c conda-forge pyspark
RUN conda install -n base -c conda-forge -c bioconda hail
RUN mamba install -n base -c conda-forge jupyterlab


# To compile and install crcmod:
RUN pip3 uninstall crcmod && pip3 install --no-cache-dir -U crcmod


COPY /entrypoint.sh /opt/conda/bin/entrypoint.sh
RUN chmod a+x /opt/conda/bin/entrypoint.sh
CMD [".", "/opt/conda/bin/entrypoint.sh"]
