ARG RELEASE=bullseye
FROM ruby:3.2.2-slim-${RELEASE} as rubyimg


RUN apt -y update; apt clean all
RUN apt -y install systemd; apt clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

ARG RUBY_PATH=/usr/local
ARG RUBY_VERSION=3.2.2
ARG NODE_MAJOR_VERSION=16

RUN apt install wget git libncurses5-dev  -y
RUN apt-get install libssl-dev
RUN apt-get install autoconf patch build-essential rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev -y
# Due to this Dockerfile being utilized within the VAEC, we needed to add the VA root Certificate Authority (CA)
# to the trusted certs and then update them - lines 47-49
COPY VA-Internal-S2-RCA1-v1.cer /etc/pki/ca-trust/source/anchors/VA-Internal-S2-RCA1-v1.cer
RUN apt --disablerepo=epel -y update ca-certificates
RUN update-ca-trust extract

#install Postgresql 12 
RUN apt-get update && \
    apt-get install -y curl ca-certificates && \
    apt-get clean
RUN install -d /usr/share/postgresql-common/pgdg
RUN curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN sh -c 'echo "deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt bullseye-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
RUN apt-get update
RUN apt-get install -y postgresql-12
RUN apt-get clean

RUN git clone https://github.com/rbenv/ruby-build.git $RUBY_PATH/plugins/ruby-build \
&&  $RUBY_PATH/plugins/ruby-build/install.sh

RUN ruby-build $RUBY_VERSION $RUBY_PATH/

RUN gem update --system

# install node
RUN apt-get update \
   && apt-get install -y ca-certificates curl gnupg \
   && mkdir -p /etc/apt/keyrings \
   && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

ARG NODE_MAJOR=16
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list

RUN  apt-get update \
     && apt-get install nodejs -y

# # install latest ImageMagick
RUN apt update && apt install -y libpng-dev libjpeg-dev libltdl-dev libfftw3-dev libdjvulibre-dev file
RUN git clone https://github.com/ImageMagick/ImageMagick.git ImageMagick \
        && cd ImageMagick \
        && ./configure \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && magick -version

CMD [ "irb" ]
