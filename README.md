## Overview

Duix.com is a PaaS platform to empower every company  through interactive digital-human services with face-to-face dialogue.

## Install

```shell
# Install Duix
npm i duix-js -S
```

## Quickstart

```js
import duix from 'duix';

const dh = duix.init(options);
dh.ask('who are you?')
```

## Options

| Name        | Type              | Description                                                  | Example                         |
| :---------- | :---------------- | :----------------------------------------------------------- | ------------------------------- |
| token       | string            | Token for this session.                                      | 666886299769D83FB...            |
| url         | string            | The URL to the region of the Duix platform.                  | https://api.us.guiji.ai         |
| container | Element           | The digital human will be presented in this DOM. | document.querySelector('#duix') |
| logger    | boolean \| string | The log level . optional false\|'debug'\|'info'\|'warn'\|'error' , default value is false . | false                           |

## Events

| Name       | Description                        |
| ---------- | ---------------------------------- |
| load       | The digital human has been loaded. |
| loading    | The digital human is loading.      |
| play       | Start playing.                     |
| pause      | Play Paused.                       |
| timeupdate | The digital human is speeking.     |
| ended      | End playing.                       |

## Method

| Name              | Description                         |
| ----------------- | ----------------------------------- |
| ask('some words') | Talk to Numbers.                    |
| destroy()         | Destroy the digital human instance. |

