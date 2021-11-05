FROM alpine as builder

ARG ARCH

RUN set -x \
	&& apk add --update ca-certificates curl zip

RUN set -x \
	&& curl -LO https://github.com/moparisthebest/static-curl/archive/refs/heads/master.zip \
	&& unzip master.zip \
	&& cd static-curl-master \
	&& ./build.sh



FROM gcr.io/distroless/base-debian11

ARG ARCH

COPY --from=builder /tmp/release/curl-$ARCH /usr/bin/curl
