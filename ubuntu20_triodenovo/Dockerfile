FROM ubuntu:20.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    less wget vim git \
    zlib1g-dev libbz2-dev \
    build-essential && \
    apt-get clean -y

COPY Triodenovo.0.06.tar.gz /opt/Triodenovo.0.06.tar.gz
RUN cd /opt && tar zxvf Triodenovo.0.06.tar.gz && rm Triodenovo.0.06.tar.gz

COPY Argument.h /opt/triodenovo.0.06/base/Argument.h

COPY tabix-0.2.4.tar.bz2 /opt/triodenovo.0.06/third/tabix-0.2.4.tar.bz2
COPY third_Makefile /opt/triodenovo.0.06/third/Makefile
RUN cd /opt/triodenovo.0.06 && make
RUN rm /opt/triodenovo.0.06/third/*.tar.*
RUN chmod -R +x /opt/triodenovo.0.06

ENV PATH /opt/triodenovo.0.06/src:$PATH
