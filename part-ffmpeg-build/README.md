# ffmpegテスト

## 使い方

### 起動

```shell
docker run -it --device=/dev/dri/card0:/dev/dri/card0 ffmpeg /bin/sh
```

### ffmpegコマンド例

```shell
/usr/local/ffmpeg/bin/ffmpeg \
    -vaapi_device /dev/dri/card0 \
    -hwaccel vaapi \
    -hwaccel_output_format vaapi \
    -i input.ts \
    -vf 'format=nv12|vaapi,hwupload,scale_vaapi=w=1280:h=720' \
    -level 41 \
    -c:v h264_vaapi \
    -aspect 16:9 \
    -qp 23 \
    -c:a copy \
    -movflags faststart \
    -vsync 1 \
    out.mp4
```
