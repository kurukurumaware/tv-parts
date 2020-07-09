# aribb25のビルド
# b25コマンドを作り出して他のDckerfileの中で利用する
FROM kurukurumaware/alpine-build-env AS B25-build

RUN set -eux && apk upgrade --update
RUN apk add --no-cache \
    ccid musl pcsc-lite-libs musl-dev pcsc-lite-dev socat 

# arib25ライブラリのインストール
WORKDIR /build/aribb25
RUN wget https://github.com/stz2012/libarib25/archive/master.zip
RUN unzip master.zip
WORKDIR /build/aribb25/libarib25-master
RUN cmake .
RUN make
RUN make install

WORKDIR /build
RUN echo /usr/local/bin/b25 > binlist
RUN extractlibrary binlist /copydir

# dockerhubでビルド後必要な部分のみ利用出来るようにする
FROM scratch
COPY --from=B25-build /copydir /copydir
