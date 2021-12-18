FROM alpine:latest AS builder

ARG REPO=https://github.com/pymumu/smartdns.git

WORKDIR /usr/src/smartdns

RUN \
        apk add --no-cache --virtual .build-deps \
            gcc \
            libc-dev \
            linux-headers \
            make \
            openssl-dev \
        && \
        apk add --no-cache --virtual .utils-deps \
            git

RUN \
        git init && git remote add origin ${REPO} \
        && git fetch --depth 1 origin master \
        && git checkout FETCH_HEAD \
        && make

FROM alpine:latest AS runner

WORKDIR /usr/src/smartdns

COPY --from=builder /usr/src/smartdns/ .
COPY Makefile .

RUN \
        apk add --no-cache --virtual .runner-deps \
            libgcc \
            openssl \
            make \
        && make install

EXPOSE 53:53/udp

ENTRYPOINT [ "/etc/init.d/smartdns","start" ]