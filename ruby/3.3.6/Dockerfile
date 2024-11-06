ARG RELEASE=bullseye
FROM ruby:3.2.5-slim-${RELEASE} as rubyimg

# Install necessary packages and clean up
RUN apt-get update &&  \
    apt-get upgrade -y && \
    apt-get install -y systemd wget git libncurses5-dev libssl-dev autoconf patch build-essential rustc libyaml-dev  \
    libreadline6-dev zlib1g-dev libgmp-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev ca-certificates sudo gnupg curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add VA root Certificate Authority (CA) to the trusted certs
COPY install-certs.sh .
RUN bash install-certs.sh && update-ca-certificates

# Install PostgreSQL 12
RUN apt-get update && \
    apt-get install -y lsb-release && \
    echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get install -y postgresql-common && \
    yes '' | /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh && \
    apt-get install -y postgresql-12

# Install Node.js using the NodeSource setup script
ARG NODE_MAJOR_VERSION=22
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_MAJOR_VERSION}.x | bash - && \
    apt-get install -y nodejs
    
# Install ImageMagick
RUN apt-get update && \
    apt-get install -y libpng-dev libjpeg-dev libltdl-dev libfftw3-dev libdjvulibre-dev file pkg-config && \
    git clone https://github.com/ImageMagick/ImageMagick.git ImageMagick && \
    cd ImageMagick && \
    ./configure --with-png=yes && \
    make && \
    make install && \
    ldconfig /usr/local/lib && \
    magick -version
CMD [ "irb" ]