FROM alpine:3.12.0 as hadolint
RUN apk add --no-cache nodejs-npm && npm install -g remark-cli remark-preset-lint-recommended
ENTRYPOINT ["/usr/bin/remark"]
