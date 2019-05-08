FROM ruby:2.5.3

RUN apt update \
        && apt-get install build-essential -y

# install latest ImageMagick
RUN wget https://www.imagemagick.org/download/ImageMagick.tar.gz \
        && tar xvzf ImageMagick.tar.gz \
        && cd ImageMagick-7.* \
        && ./configure \
        && make \
        && make install \
        && ldconfig /usr/local/lib \
        && magick -version