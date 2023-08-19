FROM alpine:3.18
MAINTAINER socketwench@deninet.com

# The following RUNs can be combined to prevent multiple unncessary layers.
# It's broken up now because this doesn't build yet.

RUN apk -U upgrade &&\
    apk add --update --no-cache \
      capnproto \
      protobuf-c && \
    apk add --update --no-cache --virtual .build-deps \
      capnproto-dev \ 
      rust \
      git \
      protobuf-c-dev \ 
      cargo && \
    git clone --recurse-submodules https://gitlab.com/veilid/veilid.git veilid-git
    
RUN cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-tools/Cargo.toml && \
    cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-cli/Cargo.toml && \
    cp veilid-git/target/debug/libveilid_tools.so /usr/lib/libveilid_tools.so && \
    cp veilid-git/target/debug/veilid-cli /usr/bin/veilid-cli

## This part has a compile error I can't figure out with ioctl. 
#RUN PROTOC=`which protoc-c` cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-core/Cargo.toml && \
#    PROTOC=`which protoc-c` cargo build --features=rt-tokio --manifest-path=veilid-git/veilid-server/Cargo.toml
#    cp veilid-git/target/debug/libveilid_core.so /usr/lib/libveilid_core.so && \
#    cp veilid-git/target/debug/veilid-server /usr/bin/veilid-server && \

RUN apk del .build-deps && \
    rm -rf /tmp/* \
           /veilid-git \
           /var/cache/apk/*
           
# Set the USER, WORKDIR, COMMAND and ENTRYPOINT here.
