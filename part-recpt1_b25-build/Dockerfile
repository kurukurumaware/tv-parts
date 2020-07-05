# mirakcのビルド
# mirakcコマンドを作り出して他のDckerfileの中で利用する
FROM kurukurumaware/alpine-build-env AS Recpt1-build

RUN apk add --no-cache ccid musl pcsc-lite-libs musl-dev pcsc-lite-dev

# 録画コマンドインストール
RUN mkdir -p /build/recpt1
WORKDIR /build/recpt1
RUN curl -fsSL https://github.com/stz2012/recpt1/tarball/master | tar -xz --strip-components=1
WORKDIR /build/recpt1/recpt1
RUN ./autogen.sh
RUN ./configure --prefix=/usr/local  --enable-b25
RUN make -j $(nproc)
RUN make install

WORKDIR /build
RUN echo /usr/local/bin/recpt1 >> binlist
RUN extractlibrary binlist /copydir

# dockerhubでビルド後必要な部分のみ利用出来るようにする
FROM scratch

COPY --from=Recpt1-build /copydir /copydir
