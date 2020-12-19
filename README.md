# 基于bilibili/ijkplayer和zhonglushu/FFmpeg
+ zhonglushu/FFmpeg的cidi分支，主要解决Illegal temporal ID in RTP/HEVC packet错误。
+ zhonglushu/ijkplayer需要增加对rtsp和H265的支持，参考CarGuo/GSYVideoPlayer的module-lite.sh脚本配置解码格式。
同时解决播放一段时间后进入缓冲导致无法继续播放的问题，以及对延时的一些优化，具体见分支ijkplayer-k0.8.8的提交3f5c7b8，
该commit去掉了buffer，仅适合一些要求实时性的场景。
+ 安卓客户端基于CarGuo/GSYVideoPlayer，配置参数如下：
```List<VideoOptionModel> list = new ArrayList<>();
VideoOptionModel videoOptionMode01 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "fast", 1);//不额外优化
list.add(videoOptionMode01);
VideoOptionModel videoOptionMode02 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "probesize", 200);//10240
list.add(videoOptionMode02);
VideoOptionModel videoOptionMode03 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "flush_packets", 1);
list.add(videoOptionMode03);
//pause output until enough packets have been read after stalling
VideoOptionModel videoOptionMode04 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "packet-buffering", 0);//是否开启缓冲
list.add(videoOptionMode04);
//drop frames when cpu is too slow：0-120
VideoOptionModel videoOptionMode05 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "framedrop", 1);//丢帧,太卡可以尝试丢帧
list.add(videoOptionMode05);
//automatically start playing on prepared
VideoOptionModel videoOptionMode06 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "start-on-prepared", 1);
list.add(videoOptionMode06);
VideoOptionModel videoOptionMode07 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_CODEC, "skip_loop_filter", 48);//默认值48
list.add(videoOptionMode07);
//max buffer size should be pre-read：默认为15*1024*1024
VideoOptionModel videoOptionMode11 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "max-buffer-size", 0);//最大缓存数
list.add(videoOptionMode11);
VideoOptionModel videoOptionMode12 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "min-frames", 2);//默认最小帧数2
list.add(videoOptionMode12);
VideoOptionModel videoOptionMode13 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "max_cached_duration", 30);//最大缓存时长
list.add(videoOptionMode13);
//input buffer:don't limit the input buffer size (useful with realtime streams)
VideoOptionModel videoOptionMode14 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "infbuf", 1);//是否限制输入缓存数
list.add(videoOptionMode14);
VideoOptionModel videoOptionMode15 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "fflags", "nobuffer");
list.add(videoOptionMode15);
VideoOptionModel videoOptionMode16 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "rtsp_transport", "tcp");//tcp传输数据
list.add(videoOptionMode16);
VideoOptionModel videoOptionMode17 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "analyzedmaxduration", 100);//分析码流时长:默认1024*1000
list.add(videoOptionMode17);
VideoOptionModel videoOptionMode18 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "mediacodec", 1);//不额外优化
list.add(videoOptionMode18);
VideoOptionModel videoOptionMode19 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "mediacodec_hevc", 1);
list.add(videoOptionMode19);
VideoOptionModel videoOptionMode20 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "mediacodec-all-videos", 1);
list.add(videoOptionMode20);
VideoOptionModel videoOptionMode21 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "mediacodec-handle-resolution-change", 0);
list.add(videoOptionMode21);
VideoOptionModel videoOptionMode22 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "mediacodec-auto-rotate", 0);
list.add(videoOptionMode22);
VideoOptionModel videoOptionMode23 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "analyzeduration", 100);
list.add(videoOptionMode23);
VideoOptionModel videoOptionMode24 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "fps", 25);
list.add(videoOptionMode24);
VideoOptionModel videoOptionMode25 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_PLAYER, "max-fps", 60);
list.add(videoOptionMode25);
VideoOptionModel videoOptionMode26 = new VideoOptionModel(IjkMediaPlayer.OPT_CATEGORY_FORMAT, "reconnect", 1);
list.add(videoOptionMode26);
GSYVideoManager.instance().setOptionModelList(list);
```
