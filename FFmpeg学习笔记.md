# 安装
`conda install FFmpeg`

> 这里安装的版本为ffmpeg 4.1.3

# 基本使用

基本的命令格式

`ffmpeg [global params] [input params] -i [input-url] [output params] [output url]
`

查看视频基本信息：

`ffmpeg -i <input>`

编码形式：

`-c: set the encoders`<br>
`-c copy: only copy bitstreams,<br>
`-c:v : set the video encoders`<br>
`-c:a : set the audio encoders`<br>
`-an: disable audio stream`<br>
`-vn: disable video stream`<br>

截取视频：
`ffmpeg -ss #starttime -i <input> -to #endtime -c copy <output>`

设置码率: bitrate = filesize / duration

`-b:v : set video constant bitrate, eg -b:v 1M `<br>

`-b:a : set audio constant bitrate `

`-crf : 设置Constant Rate factor for lib264/265,对于264而言，CRF18~28效果就很好了，如果更低的话，视频质量会更好，而压缩比也会变小，文件会变大 e.g :ffmpeg -i <input> -c:v libx264 -crf:23 -c:a acc -b:a 128k <output>`

`-qscale/-q:v  -qscale设置 Varibale Bit Rate,这里设置video质量，数值1~31,1是最好的（很少用，质量较高，文件较大），31是最差的 e.g -q:v #level`

`-qscale/-q:a` 设置audio质量，数值变化取决于编码形式

设置帧率：就是我们常见的FPS参数

`ffmpeg -i <input> -r #fps <output>`

设置缩放（Transizing)：

`ffmpeg -i <input> -vf "scale=w=320:h=240" <output>`

`ffmpeg -i <input> -vf scale=340:240,setsar1:1 <output>这里的setsar是设置样本的像素宽高比，两个命令得到的视频有所区别`

Transrating：常和transizing一起用

`ffmpeg -i <input> -minrate 964k -maxrate 3856k -bufsize 200k <output>`

运动矢量：

`ffmpeg -flag2 +export_mvs -i <input> -vf codecview=mv=pf+bf+bb <output>`	

`pf:  forward prediction mv of p pictures`

`bf:  forward prediction mv of B pictutres`

`bb:  backword prediction mv of B pictures`

转换编码（Transcoding):注意和编码与封装的区分

`ffmpeg -i <input> -c:v libx265 <output>`

转换封装格式（Transmuxing):

`ffmpeg -i <input.mp4> -c:copy <output.mkv>`

获得指定编码文件中仅仅视频流的信息（我实验感觉的）：

`ffmpeg -i <input> -c:v mpeg4 -f rawvideo <output>`










