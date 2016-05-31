FROM ubuntu

RUN apt-get update -y && \
  apt-get install -y \
    cmake \
    subversion \
    git-core \
    git-svn \
    make gcc g++ \
    libx11-dev \
    libxt-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    libfontconfig-dev \
    libxrender-dev \
    libncurses5-dev \
    wget

    

WORKDIR /usr/src

# This will download, then build zlib and openssl in the current folder
RUN wget --no-check-certificate https://gist.githubusercontent.com/jcfr/9513568/raw/0dc6707197702e3f7f6653a1940cb467378a065c/get-and-build-openssl-for-slicer.sh && \
  chmod u+x get-and-build-openssl-for-slicer.sh && \
  ./get-and-build-openssl-for-slicer.sh

## This will configure and build Qt in RELEASE against the zlib and openssl previously built
RUN wget http://download.qt.io/official_releases/qt/4.8/4.8.7/qt-everywhere-opensource-src-4.8.7.tar.gz && \
 md5=$(md5sum ./qt-everywhere-opensource-src-4.8.7.tar.gz | awk '{ print $1 }')

RUN  tar -xzvf qt-everywhere-opensource-src-4.8.7.tar.gz && \
 rm qt-everywhere-opensource-src-4.8.7.tar.gz && \
 mv qt-everywhere-opensource-src-4.8.7 qt-everywhere-opensource-release-src-4.8.7 && \
 mkdir install-prefix && \
 cd qt-everywhere-opensource-release-src-4.8.7 && \
 LD=${CXX} ./configure -prefix /usr/src/install-prefix \
   -release \
   -opensource -confirm-license \
   -no-qt3support \
   -webkit \
   -nomake examples -nomake demos \
   -openssl -I /usr/src/openssl-1.0.1h/include -L /usr/src/openssl-1.0.1h && \
  make -j$(grep -c processor /proc/cpuinfo) && \
  make install && \
  cd .. &&\
   rm -rf qt-everywhere-opensource-src-build-4.8.7 && \
  rm -rf /usr/src/install-prefix/doc 
ENV PATH /usr/src/install-prefix/bin:$PATH

