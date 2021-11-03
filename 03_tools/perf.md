### alpine and perf
    RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" | tee -a  /etc/apk/repositories
    RUN apk add --update perf
