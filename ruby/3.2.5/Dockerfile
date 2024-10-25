ARG RELEASE=bullseye
FROM ruby:3.2.5-slim-${RELEASE} as rubyimg


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
ARG RUBY_VERSION=3.2.5
ARG NODE_MAJOR_VERSION=22
RUN apt install wget git libncurses5-dev  -y
RUN apt-get install libssl-dev
RUN apt-get install autoconf patch build-essential rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev -y


# Due to this Dockerfile being utilized within the VAEC, we needed to add the VA root Certificate Authority (CA)
# to the trusted certs and then update them - lines 47-49
COPY install-certs.sh .
RUN apt-get update && \
    apt-get install  -y ca-certificates
RUN bash install-certs.sh


#install Postgresql 12
RUN apt-get install -y lsb-release
RUN echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN apt-get install sudo
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
RUN apt-get update
RUN sudo apt-get install -y  postgresql-12

RUN gem update --system


# install node
RUN apt-get update \
   && apt-get install -y curl gnupg \
   && mkdir -p /etc/apt/keyrings \
   && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
ARG NODE_MAJOR=22
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
RUN  apt-get update \
     && apt-get install nodejs -y


#install latest ImageMagick
RUN apt update && apt install -y libpng-dev libjpeg-dev libltdl-dev libfftw3-dev libdjvulibre-dev file pkg-config
RUN pkg-config --c-flags libpng
RUN git clone https://github.com/ImageMagick/ImageMagick.git ImageMagick \
        && cd ImageMagick \
        && ./configure --with-png=yes \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && magick -version
CMD [ "irb" ]
