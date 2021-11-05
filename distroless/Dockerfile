FROM debian as builder

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

RUN set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends apt-transport-https ca-certificates curl



FROM gcr.io/distroless/base-debian11

COPY --from=builder /usr/bin/curl /usr/bin/curl
