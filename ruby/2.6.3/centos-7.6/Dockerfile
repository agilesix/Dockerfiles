FROM agilesix/centos:7.6

ARG RUBY_PATH=/usr/local
ARG RUBY_VERSION=2.6.3

RUN yum install -y \
        gcc-c++ \
        wget \
        git \
        libreadline-dev \
        libncurses5-dev \
        autoconf \
        bison \
        build-essential \
        libyaml-dev \
        libffi-dev \
        libssl-dev \
        zlib1g-dev \
        libgdbm-dev \
        bzip2 \
        epel-release \
        make \
        gcc \
        openssl-devel \
        zlib-devel \
        mysql-devel \
        redis \
        sqlite-devel \
        readline-devel \
        postgresql-server \
        postgresql-contrib 
        
RUN yum install -y libpqxx-devel

RUN git clone git://github.com/rbenv/ruby-build.git $RUBY_PATH/plugins/ruby-build \
&&  $RUBY_PATH/plugins/ruby-build/install.sh

RUN ruby-build $RUBY_VERSION $RUBY_PATH/

RUN gem update --system

# install latest ImageMagick
RUN wget https://www.imagemagick.org/download/ImageMagick.tar.gz \
        && tar xvzf ImageMagick.tar.gz \
        && cd ImageMagick-7.* \
        && ./configure \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && magick -version
        
CMD [ "irb" ]
