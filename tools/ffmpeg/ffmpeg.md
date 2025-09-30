# FFmpeg 多媒体处理工具

### 将视频文件提取为音频文件

```shell
# 提取音频为 MP3
ffmpeg -i "腾讯会议 2022-12-23 11-08-07.mp4" -vn -ar 48000 -ac 2 -b:a 128k output.mp3
```

