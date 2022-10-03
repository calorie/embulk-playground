FROM openjdk:8-jre-slim-bullseye as build

RUN apt-get update -qq && apt-get install --no-install-recommends -y wget ca-certificates \
  && rm -rf /var/lib/apt/lists/*

RUN wget https://dl.embulk.org/embulk-0.9.24.jar -O /usr/local/bin/embulk \
  && chmod +x /usr/local/bin/embulk

RUN mkdir /embulk
WORKDIR /embulk

COPY Gemfile /embulk/Gemfile
COPY Gemfile.lock /embulk/Gemfile.lock
COPY .bundle/config /embulk/.bundle/config
RUN /usr/local/bin/embulk bundle

FROM gcr.io/distroless/java:8

COPY --from=build /usr/local/bin/embulk /usr/local/bin/embulk
COPY --from=build /embulk /embulk

COPY json_payload.tsv /embulk/json_payload.tsv
COPY tsv_to_es.yml /embulk/tsv_to_es.yml
