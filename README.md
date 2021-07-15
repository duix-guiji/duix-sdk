## Overview

Duix.com is a PaaS platform to empower every company  through interactive digital-human services with face-to-face dialogue.

## Install

```shell
# Install Duix
npm i duix-guiji -S
```

## Quickstart

```js
import DUIX from 'duix-guiji'

const duix = new DUIX({
    url: 'https://robot.guiji.ai/duix-cc/',
    logger: 'error',
    faceCache: {
        duration: 'auto'
    },
    container: document.querySelector('.stage'),
    robot: {
        code: 'xxxxxxxxxxxxxxxx'
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

duix.say('你好，我是硅基智能数字人，很高兴认识您。') // 用文字驱动数字人说话
duix.say('https://duix.guiji.ai/nfs/ccm-file/0c710466e703224167ead95f1fa6ef58.wav', true) // 用音频文件驱动数字人说话
```

## Options

| Name      | Type              | Description                                                  | Example                         |
| :-------- | :---------------- | :----------------------------------------------------------- | :------------------------------ |
| token     | string            | Token for this session.                                      | 666886299769D83FB...            |
| url       | string            | The URL to the region of the Duix platform.                  | https://api.us.guiji.ai         |
| container | Element           | The digital human will be presented in this DOM.             | document.querySelector('#duix') |
| logger    | boolean \| string | The log level . optional false\|'debug'\|'info'\|'warn'\|'error' , default value is false . | false                           |

## Events

| Name       | Description                        |
| :--------- | :--------------------------------- |
| load       | The digital human has been loaded. |
| loading    | The digital human is loading.      |
| play       | Start playing.                     |
| pause      | Play Paused.                       |
| timeupdate | The digital human is speeking.     |
| ended      | End playing.                       |

## Method

| Name              | Description                         |
| :---------------- | :---------------------------------- |
| ask('some words') | Talk to Numbers.                    |
| destroy()         | Destroy the digital human instance. |

