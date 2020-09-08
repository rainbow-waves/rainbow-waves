# rainbow-waves

[rainbow-waves](https://rainbow-waves.github.io/) is a wave component on Vue by [z7xisoo](https://z7xisoo.com/) in [github](https://github.com/rainbow-waves/rainbow-waves).

## Install

```sh
npm i --save rainbow-waves
```

```js
import RainbowWaves from 'rainbow-waves'
```

## Usage

```html
<rainbow-waves :config="config"></rainbow-waves>
```

## Options


- [x] config

| key|type|default|use|
| -------- |-----:|:----:|:----:|
| el|String|"rainbow-waves"| canvas id|
| new|Boolean|true|create new canvas|
| clear|Boolean|true|clear canvas when animation|
| width|Number|1920|canvas width|
| height|Number|1080|canvas height|
| direction|String|"bottom"|waves direction|
| background|Object|{...}|canvas background|


- [x] waves

| key|type|default|use|range|
| -------- |-----:|:----:|:----:|:----:|
| jitter|Number|0.04|波浪抖动频率|[0.01 - 1.00]|
| restore|Number|0.03| 波峰差值恢复速度|[0.01 - 1.00]|
| waveGap|Number|80|波浪峰差| - |
| waterGap|Number|20|水位差| - |
| waveUps|Number|6|波浪起伏频率|[1 - 10]|
| bit|Number|0.45|波浪占比|[0.01 - 1.00]|
| background|Object|{...}|canvas background|-|

- [x] background

| key|type|default|use|range|
| -------- |-----:|:----:|:----:|:----:|
| type|String|""|type|"color","image","gradient"|
| color|String Array|""|color|-|
| position|Array|""|image-gradient-positon|[0,0,width,0]|
| src|String|""|image-url| "color","image","gradient" |
| repetition|String|""|image-repetition| "color","image","gradient" |


```js
import image from "rainbow-waves.png";

background = {
  type: "gradient",
  color: ["red", "orange", "green", "blue"],
  position:[0,0,1920,1080]
};

background = {
  type: "image",
  src:image,
  repetition:"repeat", // "repeat","repeat-x","repeat-y","no-repeat"
};

background = {
  type: "color",
  color: "green",
};

```



## LICENSE

MIT

----
                
![](https://raw.githubusercontent.com/rainbow-waves/rainbow-waves.github.io/master/wave.jpg)
