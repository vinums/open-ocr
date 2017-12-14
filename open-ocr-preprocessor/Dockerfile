FROM ubuntu:14.04

ENV GO_VERSION=1.9 \
    GOROOT=/goroot \
    GOPATH=/gopath 

ENV PATH $PATH:$GOROOT/bin:$GOPATH/bin

RUN apt-get -q -y update \
	&& apt-get -y install software-properties-common \
	&& add-apt-repository universe \
	&& apt-get -q -y update

RUN apt-get -q -y install wget \
	&& apt-get -q -y install unzip \
	&& apt-get -q -y install git

RUN apt-get -q -y update \
        && apt-get -y install software-properties-common \
        && add-apt-repository universe \
        && apt-get -q -y update \
        && apt-get -q -y install libopencv-core2.4 \
        && apt-get -q -y install libopencv-core-dev \
        && apt-get -q -y install libboost1.55-all-dev \
        && apt-get -q -y install libopencv-flann2.4 libopencv-flann-dev \
        && apt-get -q -y install libopencv-imgproc2.4 libopencv-imgproc-dev \
        && apt-get -q -y install libopencv-photo2.4 libopencv-photo-dev \
        && apt-get -q -y install libopencv-video2.4 libopencv-video-dev \
        && apt-get -q -y install libopencv-features2d2.4 libopencv-features2d-dev \
        && apt-get -q -y install libopencv-objdetect2.4 libopencv-objdetect-dev \
        && apt-get -q -y install libopencv-calib3d2.4 libopencv-calib3d-dev \
        && apt-get -q -y install libopencv-ml2.4 libopencv-ml-dev \
        && apt-get -q -y install libopencv-contrib2.4 libopencv-contrib-dev \
        && apt-get -q -y install libopencv-highgui2.4 libopencv-highgui-dev

RUN mkdir -p /opt
RUN cd /opt && git clone https://github.com/tleyden/DetectText.git
RUN cd /opt/DetectText && g++ -o DetectText TextDetection.cpp FeaturesMain.cpp -lopencv_core -lopencv_highgui -lopencv_imgproc -I/opt/DetectText

RUN cd /opt/DetectText && cp DetectText /usr/local/bin

RUN apt-get update -y \
    && apt-get install --no-install-recommends -y -q curl build-essential ca-certificates git mercurial bzr \
    && mkdir /goroot && curl https://storage.googleapis.com/golang/go${GO_VERSION}.linux-amd64.tar.gz | tar xvzf - -C /goroot --strip-components=1 \
    && mkdir /gopath \
    && apt-get clean all

RUN apt-get -q -y install git \
	&& apt-get -q -y install gcc

# since we are gonna install open-ocr, we need all the tesseract / leptonica deps
RUN apt-get -q -y install libleptonica-dev \
	&& apt-get -q -y install libtesseract3 libtesseract-dev \
	&& apt-get install -q -y tesseract-ocr-eng

RUN go get -u -v github.com/tleyden/open-ocr

# build open-ocr-preprocessor binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/tleyden/open-ocr/cli-preprocessor && go build -v -o open-ocr-preprocessor && cp open-ocr-preprocessor /usr/bin

