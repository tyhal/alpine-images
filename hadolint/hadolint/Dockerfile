FROM alpine:3.12.0 as haskell_layer
RUN apk add --no-cache git ghc=8.8.3-r0 xz wget build-base make ca-certificates \
        && update-ca-certificates

ENV HADO_VER=tags/v1.18.0
RUN git clone --recursive https://github.com/lukasmartinelli/hadolint.git
WORKDIR /hadolint
RUN git checkout "$HADO_VER"

ENV STACK_VER 2.3.1
ENV STACK_DIR stack-${STACK_VER}-linux-x86_64-static
RUN wget --no-check-certificate -O  /stack.tar.gz https://github.com/commercialhaskell/stack/releases/download/v${STACK_VER}/$STACK_DIR.tar.gz
RUN tar -xvzf /stack.tar.gz
RUN chmod +x $STACK_DIR/stack
RUN $STACK_DIR/stack update
RUN $STACK_DIR/stack --system-ghc install
RUN $STACK_DIR/stack --system-ghc install ShellCheck

FROM alpine:3.12.0 as hadolint
RUN apk --no-cache add gmp libffi
COPY --from=haskell_layer /root/.local/bin/shellcheck /bin/shellcheck
COPY --from=haskell_layer /root/.local/bin/hadolint /bin/hadolint
ENTRYPOINT ["/bin/hadolint"]
