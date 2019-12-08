FROM alpine:3.10.3 as haskell_layer
RUN apk add --no-cache git ghc=8.4.3-r0 xz wget build-base make ca-certificates \
        && update-ca-certificates

ENV HADOVER=tags/v1.15.0
RUN git clone --recursive https://github.com/lukasmartinelli/hadolint.git
WORKDIR /hadolint
RUN git checkout "$HADOVER"

ENV STACK_DIR stack-1.9.1-linux-x86_64-static
RUN wget --no-check-certificate -O  /stack.tar.gz https://github.com/commercialhaskell/stack/releases/download/v1.9.1/$STACK_DIR.tar.gz
RUN tar -xvzf /stack.tar.gz
RUN chmod +x $STACK_DIR/stack
RUN $STACK_DIR/stack update
RUN $STACK_DIR/stack --system-ghc install
RUN $STACK_DIR/stack --system-ghc install ShellCheck

FROM scratch as hadolint
COPY --from=haskell_layer /root/.local/bin/shellcheck /shellcheck
COPY --from=haskell_layer /root/.local/bin/hadolint /hadolint
ENTRYPOINT ["/hadolint"]