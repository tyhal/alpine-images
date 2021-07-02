FROM alpine:3.14.0 as remark
RUN apk add --no-cache nodejs-npm && npm install -g remark-cli remark-preset-lint-recommended
ENTRYPOINT ["/usr/bin/remark"]
