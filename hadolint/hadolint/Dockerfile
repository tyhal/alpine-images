FROM tyhal/commercialhaskell-stack as hadolint-build

ENV HADO_VER=tags/v2.4.1
RUN git clone --recursive https://github.com/hadolint/hadolint.git
WORKDIR /hadolint
RUN git checkout "$HADO_VER"

RUN stack --system-ghc --compiler ghc-${GHC_VERSION} install --flag hadolint:static
RUN stack --system-ghc --compiler ghc-${GHC_VERSION} install ShellCheck

FROM alpine:3.13.5 as hadolint
RUN apk --no-cache add gmp libffi
COPY --from=hadolint-build /root/.local/bin/shellcheck /bin/shellcheck
COPY --from=hadolint-build /root/.local/bin/hadolint /bin/hadolint
ENTRYPOINT ["/bin/hadolint"]