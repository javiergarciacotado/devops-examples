FROM alpine:3.17.3

RUN apk add coreutils

CMD exec /bin/sh -c "trap : TERM INT; (while true; do sleep 1000; done) & wait"