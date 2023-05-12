FROM ruby:3.2.2-alpine3.17
RUN apk add --no-cache \
    binutils-gold \
    build-base \
    linux-headers \
    musl \
    musl-dev \
    gcc \
    g++ \
    make \
    make \
    libstdc++ \
    libffi-dev \
    libc-dev \
    curl \
    git \
    sqlite \
    sqlite-dev \
    python3 \
    nodejs \
    libxml2-dev \
    libxslt-dev \
    libgcrypt-dev \
    netcat-openbsd \
    openssl \
    pkgconfig \
    postgresql-dev \
    tzdata \
    yarn \
    libc6-compat \
    && ln -s /lib/libc.musl-aarch64.so.1 /lib/ld-linux-aarch64.so.1

# update RubyGems
RUN gem update --system 
RUN gem install bundler -v 2.4.13
WORKDIR /srv
RUN bundle config build.nokogiri --use-system-libraries
RUN gem update --system 
RUN gem uninstall sqlite3 --all
RUN gem install sqlite3
