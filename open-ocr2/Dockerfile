FROM ubuntu:16.04

ENV GO_VERSION=1.9 \
    GOROOT=/goroot \
    GOPATH=/gopath 

ENV PATH $PATH:$GOROOT/bin:$GOPATH/bin

RUN apt-get update -y \
    && apt-get install --no-install-recommends -y -q curl build-essential ca-certificates git mercurial bzr \
    && mkdir /goroot && curl https://storage.googleapis.com/golang/go${GO_VERSION}.linux-amd64.tar.gz | tar xvzf - -C /goroot --strip-components=1 \
    && mkdir /gopath \
    && apt-get clean all

# Get tesseract-ocr packages

RUN apt-get update && apt-get install -y \
  software-properties-common \
  git \
  gcc
  
RUN add-apt-repository ppa:alex-p/tesseract-ocr && apt-get update

RUN apt-get install -y \
  libleptonica-dev \
  libtesseract4 \
  libtesseract-dev \
  tesseract-ocr

# Get language data.
RUN apt-get install -y \
  tesseract-ocr-eng

RUN mkdir -p $GOPATH

# go get open-ocr
RUN go get -u -v -t github.com/tleyden/open-ocr

# build open-ocr-httpd binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/tleyden/open-ocr/cli-httpd && go build -v -o open-ocr-httpd && cp open-ocr-httpd /usr/bin

# build open-ocr-worker binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/tleyden/open-ocr/cli-worker && go build -v -o open-ocr-worker && cp open-ocr-worker /usr/bin 
