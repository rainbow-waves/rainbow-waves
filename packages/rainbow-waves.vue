<template>
  <canvas v-if="createCanvas" :id="elId"></canvas>
</template>

<script>
export default {
  name: "rainbow-waves",
  props: {
    config: {
      type: Object,
    },
    waves: {
      type: Array,
    },
  },
  computed: {
    createCanvas() {
      if (this.config && this.config.hasOwnProperty("new")) {
        return this.config.new;
      }
      return true;
    },
    elId() {
      if (this.config && this.config.hasOwnProperty("el")) {
        return this.config.el;
      }
      return "rainbow-waves";
    },
  },
  watch: {
    config: {
      handler() {
        this.$nextTick(() => {
          this.draw();
        });
      },
      deep: true,
    },
    waves: {
      handler() {
        this.$nextTick(() => {
          this.draw();
        });
      },
      deep: true,
    },
  },
  mounted() {
    if (this.createCanvas) {
      this.draw();
    } else {
      setTimeout(() => {
        this.draw();
      }, 10);
    }
  },
  methods: {
    draw() {
      // 初始化
      let t = 0;
      let canvas = null;
      let ctx = null;
      let width = 0;
      let height = 0;

      const waves = this.waves ? [...this.waves] : [];

      // 配置项
      const {
        new: create,
        el,
        width: xW,
        height: xH,
        clear,
        background,
        direction,
      } = {
        el: "rainbow-waves",
        clear: true,
        new: true,
        width: window.innerWidth,
        height: window.innerHeight,
        background: {
          type: "color",
          color: "#fff",
        },
        direction: "bottom",
        ...this.config,
      };

      // 获取 画布
      canvas = document.getElementById(el);
      // 未找到指定元素
      if (!canvas) throw new Error(`指定ID为 ${el} 的元素未找到`);
      if (!canvas) return;
      ctx = canvas.getContext("2d");
      // 配置 画布
      if (create) {
        canvas.width = width = xW;
        canvas.height = height = xH;
      } else {
        width = canvas.width;
        height = canvas.height;
      }
      // 颜色配置函数
      function setColor(background) {
        // 未找到指定元素
        if (!ctx) throw new Error(`未找到‘canvas’, getContext 错误`);
        if (!ctx) return;
        switch (background.type) {
          case "color":
            ctx.fillStyle = background.color;
            break;
          case "gradient":
            const [o1, o2, o3, o4] = background.position;
            const gradient = ctx.createLinearGradient(o1, o2, o3, o4);
            const l = background.color.length;
            background.color.forEach((x, k) => {
              gradient.addColorStop(k / l, x);
            });
            ctx.fillStyle = gradient;
            break;
          case "image":
            let img = new Image();
            img.src = background.src;
            ctx.fillStyle = ctx.createPattern(img, background.repetition);
            break;
        }
      }
      // 配置波浪
      loop();
      function loop() {
        if (clear) ctx.clearRect(0, 0, width, height);
        if (create) {
          setColor(background);
        } else {
          setColor({
            type: "color",
            color: "rgba(0,0,0,0)",
          });
        }
        ctx.fillRect(0, 0, width, height);
        if (waves && waves.length) {
          for (let i = 0; i < waves.length; i++) {
            let { jitter, restore, waveGap, waterGap, waveUps } = waves[i];
            ctx.beginPath();
            for (let e = 0; e <= 1 + 0.01; e += 0.01) {
              if (direction === "left" || direction === "right") {
                ctx.lineTo(
                  width *
                    (direction === "left"
                      ? waves[i]["bit"]
                      : Math.floor((1 - waves[i]["bit"]) * 10) / 10) +
                    (Math.sin(e * waveUps + t * jitter) * waveGap + waterGap) *
                      Math.sin(t * restore),
                  e * height
                );
              } else {
                ctx.lineTo(
                  e * width,
                  height *
                    (direction === "top"
                      ? waves[i]["bit"]
                      : Math.floor((1 - waves[i]["bit"]) * 10) / 10) +
                    (Math.sin(e * waveUps + t * jitter) * waveGap + waterGap) *
                      Math.sin(t * restore)
                );
              }
            }

            switch (direction) {
              case "left":
                ctx.lineTo(0, height);
                ctx.lineTo(0, 0);
                break;
              case "right":
                ctx.lineTo(width, height);
                ctx.lineTo(width, 0);
                break;
              case "top":
                ctx.lineTo(width, 0);
                ctx.lineTo(0, 0);
                break;
              default:
                ctx.lineTo(width, height);
                ctx.lineTo(0, height);
                break;
            }
            ctx.closePath();
            setColor(waves[i]["background"]);
            ctx.fill();
          }
          t++;
        }
        requestAnimationFrame(loop);
      }
    },
  },
};
</script>