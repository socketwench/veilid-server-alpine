FROM alpine:3.18
MAINTAINER socketwench@deninet.com

RUN apk -U upgrade &&\
    apk add --update --no-cache capnproto capnproto-dev rust git protobuf-c protobuf-c-dev && \
    git clone --recurse-submodules https://gitlab.com/veilid/veilid.git veilid-git && \
    cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-tools/Cargo.toml && \
    PROTOC=`which protoc-c` cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-core/Cargo.toml && \
    PROTOC=`which protoc-c` cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-server/Cargo.toml && \
    cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-cli/Cargo.toml && \
    cp veilid-git/target/debug/libveilid_tools.so /usr/lib/libveilid_tools.so && \
    cp veilid-git/target/debug/libveilid_core.so /usr/lib/libveilid_core.so && \
    cp veilid-git/target/debug/veilid-cli /usr/bin/veilid-cli && \
    cp veilid-git/target/debug/veilid-server /usr/bin/veilid-server && \
    rm -rf /tmp/* \
           /veilid-git \
           /var/cache/apk/*
