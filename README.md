## **概述**

​		DUIX 数字人通过 TTS、ASR、数字人克隆等技术，对人类进行虚拟仿真，打造高度拟人化、可交互的虚拟数字形象。数字人可以像人类一样倾听、理解，运用情感化的语言、表情和动作与用户交流，使数字人更加人性化。用户可以通过手机、电脑、一体机、投影或 PAD 等方式与数字人开展对话,数字人以人的说话方式与用户进行交流。DUIX提供数字人交互PASS平台能力，赋能企业及生态。
​		DUIX平台高度开放，拥有完备的生态集成体系，NLP、智能语音、数字人形象等都支持开放集成。同时 DUIX 提供高度抽象的数字人交互 SDK，易于集成，终端兼容性高，使企业更关注业务本身。 

## 安装

```shell
# Install Duix
npm i duix-guiji -S
```

## 快速开始

```js
import DUIX from 'duix-guiji'

const duix = new DUIX({
    url: 'https://robot.guiji.ai/duix-cc/',
    logger: 'error',
    container: document.querySelector('.stage'),
    robot: {
        code: 'xxxxxxxxxxxxxxxxxx'
    }
})

duix.on('load', () =>{
    console.log('load')
})

duix.on('canplaythrough', function (e) {
    console.log('canplaythrough')
    duix.play()
})

duix.on('error', function (e) {
    console.error('error', e)
})

duix.on('bodyload', function (e) {
    duix.say('你好，我是硅基智能数字人，很高兴认识您。') // 用文字驱动数字人说话
	// duix.say('https://duix.guiji.ai/nfs/ccm-file/0c710466e703224167ead95f1fa6ef58.wav', true) // 用音频文件驱动数字人说话
})

```

## 选项

以上示例中的new DUIX(options)即可得到一个DUIX实例，其中option是一个配置对象，细节如下：

| 名称               | 类型            | 描述                                                         | 默认值 | 示例                            |
| :----------------- | :-------------- | :----------------------------------------------------------- | ------ | ------------------------------- |
| container          | Element         | 数字人将渲染在这个Dom中，并占宽高均占满容器。                |        | document.querySelector('#duix') |
| logger             | boolean\|string | 日志等级。可选值 false\|'debug'\|'info'\|'warn'\|'error'     | false  | false                           |
| url                | string          | 服务器URL，您从DUIX后台获取到的服务URL.                      |        | https://api.us.guiji.ai         |
| faceCache          | object          | 缓冲相关配置                                                 |        |                                 |
| faceCache.duration | number          | 缓冲时长，单位秒。在CPU稍差的机器上可以适当增加缓冲。当缓冲加满以后会触发`canplaythrough`事件。 | 1      |                                 |
| robot              | object          | 数字人相关配置                                               |        |                                 |
| robot.code         | string          | 数字人Code。从官网后台获得。                                 |        | 205692370051410784              |
| quality            | object          | 画质相关配置。数字人解码占用一些CPU，在CPU较弱的场景下，可以调整画质以提升解码速度。 |        |                                 |
| quality.fps        | number          | 画面帧率。可选值：15\|20\|25                                 | 25     | 20                              |
| quality.isQuarter  | boolean         | 是否降低分辨率。值为true时画面分辨率降为270*480              | false  |                                 |
| body               | object          | 数字人静默视频相关配置                                       |        |                                 |
| body.autoplay      | boolean         | 当加载完身体时是否自动播放，如置为false，可在 `bodyload` 事件触发后调用 duix.playSilence() 方法主动播放 | true   |                                 |

## 方法

#### DUIX(options)构造函数

参数见如上表格

#### say(words,[isVoice = false])

驱动数字人说话，支持文本驱动和音频文件驱动。调用该方法后，即刻开始加载资源，缓冲加载满以后触发`canplaythrough`事件。该方法调用时要确保数字人的静默视频已经加载完成，也就是`bodyload`事件已经触发。

##### 参数：

###### words

需要数字人说的话，可以是文本，比如“你好，我是硅基智能数字人，很高兴认识您。”；也可以是一个音频URL，比如`https://www.xxxx.com/cdn/abcd.wav`。

###### isVoice 

可选参数，表示`words`参数是不是一个音频。true表示是一个音频，默认false。

#### playSilence()

播放静默视频。`options.body.autoplay = false`时，用此方法播放静默视频。

#### play()

播放数字人，一般在`canplaythrough`事件中调用此方法。

#### pause()

暂停数字人。暂停后数字人不说话，画面切换成静默视频。

#### resume()

由暂停恢复播放。

#### stop（）

停止播放视频。

#### on(eventname,callback)

监听事件。

##### 参数：

###### eventname

事件名称，详见下方表格。

###### callback

回调函数。

#### getCanvas()

获取内部的canvas，可对canvas做一些高级开发。

#### getAudioContext()

获取内问音频，可以基于此做一些音频可视化等。

## 事件

| 名称           | 描述                                                         |
| :------------- | :----------------------------------------------------------- |
| bodyload       | 静默视频加载完成。只有此事件以后才能调用`duix.say(xxx)`。    |
| bodyprogress   | 静默视频加载进度，可利用此事件做一个加载动画。               |
| canplaythrough | 缓冲已满，可以开始播放。                                     |
| load           | 本轮数字人说话内容已加载完成。数字人资源是懒加载的，不播放（`duix.play()`）可能永远也不会触发此事件。 |
| play           | 数字人开始播放。                                             |
| pause          | 数字人播放已暂停。                                           |
| timeupdate     | 数字人正在播放，播放每一帧时都触发此事件。                   |
| ended          | 数字个播放结束。                                             |
| error          | DUIX异常。                                                   |

### 版本记录：
0.0.43
1.新增从AudioContext获取MediaStream的方法getAudioDest

0.0.42
1. Request.js => getArrayBuffer 添加主动断开请求的方法
2. DigitalHuman.js => _sayVoice 添加判断网络取消时的return
3. DigitalHuman.js => stop 添加cancel方法 防止stop后网络请求才成功导致stop失败

0.0.41
1. Request.js 添加axios超时时间
2. Request.js => getArrayBuffer 添加音频请求失败返回 &&
DigitalHuman.js => _sayVoice 添加判断 音频请求失败时调用event &&
DUIX.js => 添加新的事件 audioFailed 音频请求失败时event

0.0.40

1. 修复bug DigitalHuman.js line:166 & 169 事件名错误导致wsClose wsError不触发的bug
2. 修改webpack配置 默认输出一次sdk版本 方便开发和生产环境的调试

0.0.39

1. 新增暂停(pause)，恢复(resume)方法。
2. 修复偶现的吞字现象。
3. 播放结束不再触发puase事件，只触发ended 事件。
4. 新增功能，页面不可见时暂停画面和声音，恢复可见时继续播放。

0.0.38

1. 修复偶现的调用say方法后，加载卡住，不能播放的bug。
2. 新增功能。当options.body.autoplay=false时，调用say不自动播放静默。

0.0.37

1. 新增 getCanvas() 方法。
2. 新增 getAudioContext() 方法。

0.0.36

1. 修改启动方式,现在以ip形式系统 手机上可以正常访问测试
2. AIFace.js reconnectInterval修改为1 开启断线重连
3. Bug修复 AIFace.js line:48 close => onClose

0.0.34：

1. 新增 wsClose 事件，AIFace 连接关闭事件。
2. 新增 wsError 事件, AIFace 连接错误事件。

0.0.33：

1. 静默视频正放/倒放切换，解决静默首尾不相接（如约旦男）时静默视频跳动的问题。
2. 删除一些调试日志。
3. 修复不触发load事件的bug

0.0.32

1. 修复过短的音频不能触发 canplaythrough 事件的bug。

0.0.31

1. 进一步优化客户端缓冲策略，降低内存占用，现在约旦模型内存占用稳定在700M左右。
2. 修复一些bug

0.0.30

1. 修改客户端缓冲策略，降低客户端内存占用。
2. 新增配置 options.body.autoplay ，用于控制静默视频加载完是否自动播放。默认为true，如置为false，可在 bodyload 事件触发后调用 duix.playSilence() 方法主动播放。
3. 优化TTS缓存方案，现在缓存可以保留更长时间。

0.0.27

1. 新增配置body.autoplay控制身体加载完后是否自动播放。
2. 删除实时贴图的代码，必须走缓冲，缓冲大小可设置为0。
3. 默认缓冲策略修改为auto，由第一次加载人脸的开始半秒加载速度预测缓冲大小。
4. 调整解码间隔时间，降低CPU的瞬间消耗，解决部分手机CPU瞬间占用过高导致页面强行刷新的问题。

0.0.26

1. 修复不传 quality.fps 和 quality.quarter时的报错。

2. 新增bodyprocess事件用于通知身体加载进度。
