FROM ruby:3.0.2

ENV LANG C.UTF-8
ENV TZ Asia/Tokyo

# update openssl
#RUN wget https://www.openssl.org/source/openssl-1.1.0f.tar.gz && \
#    tar xzvf openssl-1.1.0f.tar.gz && \
#    cd openssl-1.1.0f && \
#    ./config -Wl,--enable-new-dtags,-rpath,'$(LIBRPATH)' && \
#    make && sudo make install


RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends sudo curl apt-transport-https wget build-essential libpq-dev nodejs default-mysql-client

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get install --no-install-recommends -y yarn

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /app
WORKDIR /app
COPY Gemfile /Gemfile
COPY Gemfile.lock /Gemfile.lock
RUN bundle install
RUN rake db:create
RUN rake db:migrate
COPY . /app

COPY docker/app/entrypoint.sh /usr/bin/
COPY scripts /app/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

