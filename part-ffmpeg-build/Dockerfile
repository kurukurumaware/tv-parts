# ffmpegのビルド
# ffmpegコマンドを作り出して他のDckerfileの中で利用する
#
# 使用の際には最終Dockerイメージにlibva-intel-driverのインストールが必要
#
FROM kurukurumaware/alpine-build-env AS Ffmpeg-build

ENV FFMPEG_VERSION=4.2.3 \
    PREFIX=/usr/local \
    LD_LIBRARY_PATH=/usr/lib

RUN set -eux \
    && apk update
RUN apk add --no-cache \
#    autoconf \
#    automake \
#    pkgconf \
#    ca-certificates \
#    curl \
#    gcc \
    coreutils \
    freetype-dev \
    build-base \
    lame-dev \
    libtool \
    libogg-dev \
    libass \
    libass-dev \
    libvpx-dev \
    libvorbis-dev \
    libwebp-dev \
    libtheora-dev \
    libva-dev \
    libvdpau-dev \
    texinfo \
    openssl-dev \
    opus-dev \
    rtmpdump-dev \
    x264-dev \
    x265-dev \
    yasm \
    libva-intel-driver

RUN echo http://dl-cdn.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories && \
    apk add --no-cache fdk-aac-dev

RUN mkdir -p /build/aribb24
WORKDIR /build/aribb24
RUN curl -fsSL https://github.com/nkoriyama/aribb24/tarball/master \
    | tar -xz --strip-components=1
RUN autoreconf -fiv
RUN ./configure --prefix=/usr/local --enable-static --disable-shared
RUN make
RUN make install

RUN mkdir -p /build/ffmpeg
WORKDIR /build/ffmpeg
RUN curl -fsSL http://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.gz \
    | tar -xz --strip-components=1

RUN PKG_CONFIG_PATH=/usr/local/lib/pkgconfig ./configure \
    --enable-version3 \
    --enable-gpl \
    --enable-nonfree \
    --enable-small \
    --enable-libmp3lame \
    --enable-libx264 \
    --enable-libx265 \
    --enable-libvpx \
    --enable-libtheora \
    --enable-libvorbis \
    --enable-libopus \
    --enable-libfdk-aac \
    --enable-libass \
    --enable-libwebp \
    --enable-librtmp \
    --enable-libaribb24 \
    --enable-postproc \
    --enable-avresample \
    --enable-libfreetype \
    --enable-openssl \
    --disable-debug \
    --disable-doc \
    --disable-ffplay \
    --extra-cflags="-I${PREFIX}/include" \
    --extra-ldflags="-L${PREFIX}/lib" \
    --extra-libs="-lpthread -lm" \
    --prefix="${PREFIX}"
RUN make -j $(nproc)
RUN make install
RUN make distclean

RUN echo /usr/local/bin/ffmpeg >> binlist
RUN echo /usr/local/bin/ffprobe >> binlist
RUN extractlibrary binlist /copydir

# dockerhubにビルドおまかせ、生成物のみ取り出す。
FROM scratch

COPY --from=Ffmpeg-build /copydir /copydir
