FROM ruby:2.6.5

RUN apt update \
        && apt-get install build-essential curl gnupg apt-utils -y

RUN curl -sL https://deb.nodesource.com/setup_11.x  | bash -
RUN apt-get -y install nodejs

# install latest ImageMagick
RUN wget https://imagemagick.org/download/ImageMagick.tar.gz \
        && tar xvzf ImageMagick.tar.gz \
        && cd ImageMagick-7.* \
        && ./configure \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && magick -version
