FROM azul/zulu-openjdk-debian:8-jre as build

ARG GITHUB_USER
ARG GITHUB_TOKEN

RUN apt-get update -qq && apt-get install --no-install-recommends -y wget ca-certificates \
  && rm -rf /var/lib/apt/lists/*

RUN wget https://dl.embulk.org/embulk-0.9.24.jar -O /usr/local/bin/embulk \
  && chmod +x /usr/local/bin/embulk

RUN mkdir /embulk
WORKDIR /embulk

COPY Gemfile /embulk/Gemfile
COPY Gemfile.lock /embulk/Gemfile.lock
RUN /usr/local/bin/embulk bundle config --local https://rubygems.pkg.github.com/st-tech ${GITHUB_USER}:${GITHUB_TOKEN}
RUN /usr/local/bin/embulk bundle install --path .

FROM gcr.io/distroless/java:8

COPY --from=build /usr/local/bin/embulk /usr/local/bin/embulk
COPY --from=build /embulk /embulk

COPY config /embulk/config
