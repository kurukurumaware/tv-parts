# mirakcのビルド
# mirakcコマンドを作り出して他のDckerfileの中で利用する
FROM kurukurumaware/alpine-build-env AS Recpt1-build

RUN set -eux && apk upgrade --update
RUN apk add --no-cache \
    ccid musl pcsc-lite-libs musl-dev pcsc-lite-dev

# arib25ライブラリのインストール
WORKDIR /build/aribb25
RUN curl -fsSL https://github.com/stz2012/libarib25/tarball/master | tar -xz --strip-components=1
RUN cmake .
RUN make
RUN make install

# 録画コマンドインストール
WORKDIR /build/recpt1
RUN curl -fsSL https://github.com/stz2012/recpt1/tarball/master | tar -xz --strip-components=1
WORKDIR /build/recpt1/recpt1
RUN ./autogen.sh
RUN ./configure --prefix=/usr/local --enable-b25
RUN make -j $(nproc)
RUN make install

WORKDIR /build
RUN echo /usr/local/bin/b25 > binlist
RUN echo /usr/local/bin/recpt1 >> binlist
RUN extractlibrary binlist /copydir

# dockerhubでビルド後必要な部分のみ利用出来るようにする
FROM scratch
COPY --from=Recpt1-build /copydir /copydir
