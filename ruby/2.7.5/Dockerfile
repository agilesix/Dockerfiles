FROM agilesix/centos:7.6

RUN yum -y update; yum clean all
RUN yum -y install systemd; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

ARG RUBY_PATH=/usr/local
ARG RUBY_VERSION=2.7.5
ARG NODE_MAJOR_VERSION=16

RUN yum install -y yum-plugin-ovl
RUN yum install -y vim-X11 vim-common vim-enhanced vim-minimal
RUN yum install -y centos-release-scl-rh
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
        readline-devel
COPY VA-Internal-S2-RCA1-v1.cer /etc/pki/ca-trust/source/anchors/VA-Internal-S2-RCA1-v1.cer
RUN yum --disablerepo=epel -y update ca-certificates
RUN update-ca-trust extract
RUN wget https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
RUN yum install -y pgdg-redhat-repo-latest.noarch.rpm
RUN yum install -y readline-devel posgtgresql-server postgresql-contrib postgresql-devel
RUN yum install -y postgresql12-server postgresql12-contrib postgresql12-devel


RUN git clone https://github.com/rbenv/ruby-build.git $RUBY_PATH/plugins/ruby-build \
&&  $RUBY_PATH/plugins/ruby-build/install.sh

RUN ruby-build $RUBY_VERSION $RUBY_PATH/

RUN gem update --system

# install node
RUN curl -fsSL https://rpm.nodesource.com/setup_${NODE_MAJOR_VERSION}}.x | bash - \
        && yum install -y nodejs \
        && curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo \
        && rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg \
        && yum install -y yarn

# # install latest ImageMagick
RUN yum install -y libpng-devel libjpeg-devel libtool-ltdl-devel fftw3-devel djvulibre-devel libltdl-dev file-devel
RUN git clone https://github.com/ImageMagick/ImageMagick.git ImageMagick \
        && cd ImageMagick \
        && ./configure \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && magick -version

CMD [ "irb" ]
