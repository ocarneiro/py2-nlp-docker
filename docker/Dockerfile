# Dockerfile for Modern NLP with Python 2

# Inspired by Patrick Harrison's talk
# https://github.com/skipgram/modern-nlp-in-python/blob/master/executable/Modern_NLP_in_Python.ipynb

# Based on Continuum's Anaconda image
# https://hub.docker.com/r/continuumio/anaconda/

FROM debian:8.5

MAINTAINER Otavio Carneiro <otavio@carneiro.blog.br>

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda2-4.3.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

ENV PATH /opt/conda/bin:$PATH

RUN /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && mkdir /opt/notebooks"

RUN conda install -y spacy && python -m spacy.en.download

RUN conda install -y gensim

RUN conda install -y -c memex pyldavis &&  \
	conda install -y ipykernel &&  \
	ipython kernel install --user

CMD ["jupyter", "notebook", "--no-browser", "--ip='*'", "--port=8888",  \
     "--notebook-dir=/opt/notebooks"]

ENTRYPOINT [ "/usr/bin/tini", "--" ]

