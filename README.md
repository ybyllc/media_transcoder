# media_transcoder
将一个目录的视频图片文件进行转码。  
原本只是个人使用的程序，贪快代码写得并不规范，轻喷。  
而且很有可能不会更新了。。  

# 使用场景
设计初衷是为了尽量保存质量的情况下压缩视频图片以节省储存空间。  

* 平板放了一堆学习视频，很多视频文件虽然分辨率不低，实际上因为拍摄时的各种因素显示效果并不好，没必要用到那么大的分辨率，压缩了也没什么影响。
* NAS或者个人电脑上放了一堆小姐姐。。不舍得删但其实很少回头看，能尽量保存质量压缩的话能存更多。。。
* ...

# 运行结果

* 指定目录及其子目录下的所有视频和图片文件的同目录生成转码后的文件，文件名末尾增加_transcoded以区分，可设置删除原文件，可设置生成日志文件。  
* 转码效果根据设置的参数而定。默认配置下，7G的av可以压缩到600mb左右，而且画质还不错，真香。分辨率不高的视频基本能做到视频大小减半。
* 运行过程将非常耗时，风扇狂转。。

# 设计思路

* 通过python调用命令行，使用[FFmpeg](https://github.com/FFmpeg/FFmpeg)进行转码。
* 检测视频分辨率和fps，默认压缩到原比例1280宽度和30fps（如果超过，否则按照原分辨率和fps）。
* 默认使用libx265，压缩率非常高，且画质基本不变。
* 视频默认转码为mp4文件，图片默认jpeg文件，方便各种播放器解码。
* 自动检测目录及子目录下的所有媒体文件，后台自动转码。

# 缺点

* 为了追求质量使用纯cpu转码，默认又使用libx265，效率全靠cpu
* 因为subprocess的一个坑，想弄单个文件的进度条非常麻烦，服务器上使用没必要所以没有去处理，所以现在只有总的进度条。也就是说，可以看得到处理了多少个文件，但是看不到现在这个文件处理了多少。。

# 使用方法

## 下载文件
使用linux的大佬不需要说啥了。  
使用windows的同学可以在[release页面](https://github.com/chuanzz/media_transcoder/releases"%3Ehttps://github.com/chuanzz/media_transcoder/releases)下载已经使用pyinstaller生成的exe文件。  
android的话安装python3的ide应该也能用，但不建议。。  

## 修改配置
配置文件为 config.ini，参考以下解释修改即可。  
为了省事我直接用python代码的格式写ini，不规范，但是真的简便。。  

### trans_dir 
```
trans_dir = '.'
```
需要转码的目录，相对路径或绝对路径均可。  
默认为运行文件所在目录。  

### video_format 
```
video_format = (
    ('.mp4', '.mkv', '.avi', '.wmv','.mov', '.rmvb', '.rm', '.3gp'),
    ('.mp4')
)
```
转码视频格式后缀，**小写**  
上一个括号为转码前，后为转码后  
默认将mp4,mkv,avi等转码为mp4文件  
将上一个空格留空则不处理视频文件  

### img_format 
```
img_format = (
    ('.jpg', '.jpeg', '.png', '.bmp'),
    ('.jpeg')
)
```
转码图片格式后缀，**小写**  
上一个括号为转码前，后为转码后  
默认将jpg,jpeg,png,bmp转码为jpeg文件  
将上一个空格留空则不处理图片文件  

### file_size_line
```
file_size_line = 0
```
转码文件大小界限（GB），超出此界限则转码，否则跳过  
默认任意大小都进行转码  

### fps_line
```
fps_line = 30
```
转码视频fps界限，超出则转换为此界限  
默认fps超出30则转换为30，否则不变  

### width_line
```
width_line = 1280
```
转码视频宽度界限，超出则转换为此界限  
  
### trans_format
```
trans_format = 'libx265'
```
使用的视频编码器，常用libx265,libx264  
前者压缩率高，但转码会非常耗时间  
后者压缩率相对较低，但转码也会较快  
前者转码得到的文件大小大概为后者的一半  

### ffmpeg_dir
```
ffmpeg_dir = './ffmpeg/'
```
ffmpeg运行文件所在目录，已安装可设置为""  
默认为同目录下的ffmpeg文件夹  
需包含ffmpeg和ffprobe运行文件  

### remove_raw_file
```
remove_raw_file = False
```
转码完成后是否删除原文件  
默认不删除，修改为True则删除。**谨慎开启**。  

### generate_log_file
```
generate_log_file = False
```
是否产生日志文件  
日志文件则为ffmpeg程序的所有输出  
默认不产生  
修改为True则会在每个视频文件的同目录产生同文件名+transcoded.log  

# 联系方式
酷安 @大草莓派  
对本程序有建议可提交issue或酷安联系我  

# 捐献通道
写这么水没脸要捐献。。  
