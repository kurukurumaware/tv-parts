# arib-b25-stream-testのビルド
# arib-b25-stream-testコマンドを作り出して他のDckerfileの中で利用する
FROM node:14.4.0-alpine3.12 AS arib-b25-stream-test-build
WORKDIR /root

RUN set -x
RUN apk upgrade --update
RUN apk add bash curl make g++ pkgconf
RUN apk add pcsc-lite pcsc-lite-libs pcsc-lite-dev
RUN npm install arib-b25-stream-test -g --unsafe 
ADD https://raw.githubusercontent.com/kurukurumaware/extlibcp/master/extlibcp /usr/local/bin
RUN chmod +x /usr/local/bin/extlibcp

RUN extlibcp /usr/local/bin/arib-b25-stream-test /copydir

# dockerhubでビルド後必要な部分のみ利用出来るようにする
FROM scratch

COPY --from=arib-b25-stream-test-build /copydir /copydir

SHELL ["/bin/sh","-l","-c"]