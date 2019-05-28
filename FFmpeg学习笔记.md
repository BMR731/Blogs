# 安装
`conda install FFmpeg`

# 基本使用

基本的命令格式

`ffmpeg [global params] [input params] -i [input-url] [output params] [output url]
`


`-c: set the encoders`<br>
`-c copy: only copy bitstreams`<br>
`-c:v : set the video encoders`<br>
`-c:a : set the audio encoders`<br>
`-an: disable audio stream`<br>
`-vn: disable video stream`<br>

截取视频：
`ffmpeg -ss 2.5 -i <input> -to 10 -c copy <output>`

设置码率:<br>
`-b:v : set video bitrate, eg -b:v 1M `<br>


