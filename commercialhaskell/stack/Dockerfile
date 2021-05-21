FROM alpine:3.13.5 as stack
ENV GHC_VERSION=8.8.4
RUN apk add --no-cache git ghc=${GHC_VERSION}-r0 xz wget build-base make ca-certificates \
        && update-ca-certificates

ENV STACK_VER 2.5.1
ENV STACK_DIR stack-${STACK_VER}-linux-x86_64
ENV STACK_REPO https://github.com/commercialhaskell/stack
RUN wget --no-check-certificate -qO- "${STACK_REPO}/releases/download/v${STACK_VER}/$STACK_DIR.tar.gz" \
    | tar -xz "${STACK_DIR}/stack"
RUN mv "${STACK_DIR}/stack" /usr/local/bin/
RUN chmod +x /usr/local/bin/stack
RUN mkdir /etc/stack/ && echo "system-ghc: true" >/etc/stack/config.yaml