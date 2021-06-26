# syntax=docker/dockerfile:1.2

ARG VERSION=latest
ARG REPO=tyhal
FROM ${REPO}/stack:${VERSION} as hadolint-build

ENV HADO_VER=2.5.0
ADD https://github.com/hadolint/hadolint/archive/refs/tags/v${HADO_VER}.tar.gz /tmp
RUN tar -xzf /tmp/v${HADO_VER}.tar.gz -C /
WORKDIR /hadolint-${HADO_VER}

RUN --mount=type=cache,target=/root/.stack \
    stack --system-ghc --compiler ghc-${GHC_VERSION} install --flag hadolint:static

RUN --mount=type=cache,target=/root/.stack \
    stack --system-ghc --compiler ghc-${GHC_VERSION} install ShellCheck

FROM alpine:3.13.5 as hadolint
RUN apk --no-cache add gmp libffi
COPY --from=hadolint-build /root/.local/bin/shellcheck /bin/shellcheck
COPY --from=hadolint-build /root/.local/bin/hadolint /bin/hadolint
ENTRYPOINT ["/bin/hadolint"]
