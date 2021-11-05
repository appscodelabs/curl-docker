FROM alpine as builder

ARG TARGETARCH

RUN set -x \
	&& apk add --update ca-certificates curl zip

RUN set -x \
	&& curl -LO https://github.com/moparisthebest/static-curl/archive/refs/heads/master.zip \
	&& unzip master.zip \
	&& cd static-curl-master \
	&& ARCH=${TARGETARCH} ./build.sh



FROM gcr.io/distroless/static-debian11

ARG TARGETARCH

COPY --from=builder /tmp/release/curl-$TARGETARCH /usr/bin/curl
