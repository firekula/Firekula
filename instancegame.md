# 小游戏

## QQ小游戏

* 远程加载资源有不支持的文件格式，下载不支持的格式会报错 Download Fielded
* CocosCreator在QQ小游戏里获取环境是安卓，要加判断
* CocosCreator发布QQ小游戏流程：发布微信小游戏，用VS Code打开微信小游戏文件夹，全局搜索'wx.'替换成'qq.'  saveFile前需改成fs\(和微信不一样\)
* qq小程序开发者工具打开工程会报错时更新到最新版即可

## 字节跳动小游戏

* CocosCreator直接发布微信小游戏即可

## OPPO小游戏

* OPPO小游戏发布需要将录屏和更多游戏按钮去掉，在结算页添加结算页互推
* 远程音频下载后用本地地址或者直接使用远程地址的音效不能用cocos自己的音频引擎播放，需要使用oppo的`InnerAudioContext`来播放

  ```typescript
    //官方示例
    var audio = qg.createInnerAudioContext()
    audio.loop = true
    audio.volume = 0.7
    audio.autoplay = false
    var playSound = function() {
    audio.play()
    audio.offCanplay(playSound)
    }
    audio.onCanplay(playSound)
    audio.src = 'res/demo.mp3'
  ```

* 音频播放在切后台的时候调用暂停音频的话，在切前台的时候有可能音频还会自动播放，但是音频当前状态还是暂停，这种时候可以在且前台先调用`audio.play()`再调用`audio.puase()`来暂停

## VIVO小游戏

* VIVO小游戏的远程音频需同OPPO一样处理
* VIVO小游戏在游戏里播放`cc.AudioClip`会出现没有声音的问题，需要修改源码：

  ```javascript
    //调整下 [creator目录]/resource/builtin/vivo-adapter/engien/jsb-audio.js 的接口
    //cc.audioEngine.playEffect 为
    cc.audioEngine.playEffect = function (filePath, loop) {
        return cc.audioEngine.play(filePath, loop || false, _effect.volume);
    };
  ```

## 小程序SDK接入

* 部分版本main.js如果开了md5的话不能设置模板，所以有引用js的话每次出包都要记得加上
