# aribb25のビルド
# b25コマンドを作り出して他のDckerfileの中で利用する
FROM kurukurumaware/alpine-build-env

RUN set -eux && apk upgrade --update
RUN apk add --no-cache \
    ccid musl pcsc-lite-libs musl-dev pcsc-lite-dev socat 

# arib25ライブラリのインストール
WORKDIR /build/aribb25
RUN curl -fsSL https://github.com/stz2012/libarib25/archive/master.tar.gz | tar -xz --strip-components=1
RUN cmake . \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DCMAKE_INSTALL_LIBDIR=/usr/local/lib
RUN make
RUN make install

RUN extlibcp /usr/local/bin/b25 /copydir

# dockerhubでビルド後必要な部分のみ利用出来るようにする
FROM scratch
COPY --from=0 /copydir /copydir
