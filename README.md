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
| width|Number|1920|canvas width|
| height|Number|1080|canvas height|
| backgroundColor|String|"#fff"|canvas background-color|
| waves|Array|[{...}]|waves|




- [x] waves

| key|type|default|use|range|
| -------- |-----:|:----:|:----:|:----:|
| color|String|"blue"|wave color  | - |
| jitter|Number|0.04|波浪抖动频率|[0.01 - 1.00]|
| restore|Number|0.03| 波峰差值恢复速度|[0.01 - 1.00]|
| waveGap|Number|80|波浪峰差| - |
| waterGap|Number|20|水位差| - |
| waveUps|Number|6|波浪起伏频率|[1 - 10]|
| waveHeight|Number|0.45|波浪占比|[0.01 - 1.00]|

## LICENSE

MIT

----
                
![](https://raw.githubusercontent.com/rainbow-waves/rainbow-waves.github.io/master/wave.jpg)
