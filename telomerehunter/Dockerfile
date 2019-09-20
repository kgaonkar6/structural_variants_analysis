FROM centos:centos6

MAINTAINER KSGaonkar

# Install yum dependencies
RUN yum -y update && \
    yum groupinstall -y development && \
    yum install -y \
    bzip2-devel \
    git \
    hostname \
    openssl \
    openssl-devel \
    sqlite-devel \
    sudo \
    tar \
    wget \
    zlib-dev

# Install python2.7
RUN cd /tmp && \
    wget https://www.python.org/ftp/python/2.7.15/Python-2.7.15.tgz && \
    tar xvfz Python-2.7.15.tgz && \
    cd Python-2.7.15 && \
    ./configure --prefix=/usr/local && \
    make && \
    make altinstall

# Install setuptools + pip
RUN cd /tmp && \
    wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz && \
    tar -xvf setuptools-1.4.2.tar.gz && \
    cd setuptools-1.4.2 && \
    python2.7 setup.py install && \
    curl https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py | python2.7 - && \
    pip install virtualenv

##############
#HTSlib 1.3.2#
##############
ENV HTSLIB_INSTALL_DIR=/opt/htslib

WORKDIR /tmp
RUN wget https://github.com/samtools/htslib/releases/download/1.3.2/htslib-1.3.2.tar.bz2 && \
    tar --bzip2 -xvf htslib-1.3.2.tar.bz2

WORKDIR /tmp/htslib-1.3.2
RUN ./configure  --enable-plugins --prefix=$HTSLIB_INSTALL_DIR && \
    make && \
    make install && \
    cp $HTSLIB_INSTALL_DIR/lib/libhts.so* /usr/lib/

################
#Samtools 1.3.1#
################
ENV SAMTOOLS_INSTALL_DIR=/opt/samtools

WORKDIR /tmp
RUN wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2 && \
    tar --bzip2 -xf samtools-1.3.1.tar.bz2

WORKDIR /tmp/samtools-1.3.1
RUN ./configure --with-htslib=$HTSLIB_INSTALL_DIR --prefix=$SAMTOOLS_INSTALL_DIR && \
    make && \
    make install

WORKDIR /
RUN rm -rf /tmp/samtools-1.3.1

RUN pip install pysam==0.9.0
RUN pip install PyPDF2==1.26.0
RUN pip install telomerehunter==1.1.0


