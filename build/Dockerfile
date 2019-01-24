# Build Parity in a stock alpine builder container
FROM alpine:edge as builder

WORKDIR /usr/src/app

ARG BRANCH=clique-v3

RUN apk --no-cache add git rust cargo make openssl-dev eudev-dev linux-headers g++ cmake perl musl-dev
RUN git clone -b ${BRANCH} https://github.com/goerli/parity-goerli.git && cd parity-goerli && \
    cargo build --release --features final

# Pull Parity into a second stage deploy alpine container
FROM alpine:3.8

RUN apk --no-cache add ca-certificates libgcc libstdc++ eudev

COPY --from=builder /usr/src/app/parity-goerli/target/release/parity /usr/local/bin

ENTRYPOINT ["parity","--base-path","/goerli","--chain","goerli","--jsonrpc-port","8545","--jsonrpc-interface","all","--jsonrpc-hosts","all","--jsonrpc-cors","all","--ws-interface","0.0.0.0","--ws-port","8546","--ws-origins","all","--ws-hosts","all"]
