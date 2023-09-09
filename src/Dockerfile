# This first stage builds the binaries.
FROM alpine:3.18
MAINTAINER socketwench@deninet.com

RUN apk -U upgrade &&\
    apk add --update --no-cache \
      capnproto \
      protobuf-c \
      capnproto-dev \ 
      rust \
      git \
      protobuf-c-dev \ 
      cargo && \
    git clone --recurse-submodules https://gitlab.com/ineiti/veilid.git --branch target_env_musl veilid-git && \
    PROTOC=`which protoc-c` cargo build --features=rt-tokio --manifest-path=veilid-git/Cargo.toml --release --bin veilid-server && \
    PROTOC=`which protoc-c` cargo build --features=rt-tokio --manifest-path=veilid-git/Cargo.toml --release --bin veilid-cli
           
# This one provides the minimum necessary to run the server.
FROM alpine:3.18
RUN apk add libgcc && \
    adduser -h /etc/veilid-server -D veilid && \
    mkdir -p -m 755 /var/db/veilid-server && \
    chown veilid:veilid -R /var/db/veilid-server && \
    rm -rf /tmp/* /var/cache/apk/*

COPY --from=0 /veilid-git/target/release/veilid-server /bin/veilid-server
COPY --from=0 /veilid-git/target/release/veilid-cli /bin/veilid-cli
COPY veilid-server.conf /etc/veilid-server/veilid-server.conf

RUN chmod 755 /bin/veilid-server /bin/veilid-cli && \
    chmod 644 /etc/veilid-server/veilid-server.conf

USER veilid
WORKDIR /etc/veilid-server
EXPOSE 5150

CMD ["/bin/veilid-server"]

