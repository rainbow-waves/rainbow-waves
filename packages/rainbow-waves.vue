<template>
  <div v-if="createCanvas" class="rainbow-waves">
    <canvas :id="elId"></canvas>
  </div>
</template>

<script>
let canvas = null;
let ctx = null;
let t = 0;
let width = 0;
let height = 0;

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
      return this.config ? this.config.new || true : true;
    },
    elId() {
      return this.config ? this.config.el || "rainbow-waves" : "rainbow-waves";
    },
  },
  watch: {
    config: {
      handler() {
        this.$nextTick(() => {
          this.drow();
        });
      },
      deep: true,
    },
    waves: {
      handler() {
        this.$nextTick(() => {
          this.drow();
        });
      },
      deep: true,
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.drow();
    });
  },
  beforeDestroy() {
    if (ctx) ctx.rect(0, 0, width, height);
    canvas = null;
    ctx = null;
    t = 0;
    width = 0;
    height = 0;
  },
  methods: {
    drow() {
      // 重置画布
      if (ctx) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx = null;
      }

      // 初始化
      t = 0;
      width = 0;
      height = 0;

      // 配置项
      const { new: create, el, width, height, clear, background } = {
        el: "rainbow-waves",
        clear: true,
        new: true,
        width: 1920,
        height: 1000,
        background: {
          type: "color",
          color: "#fff",
          // position
          // src
          // repetition
        },
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
        canvas.width = width;
        canvas.height = height;
        width = width;
        height = height;
      }
      // 颜色配置函数
      function setColor(background) {
        // 未找到指定元素
      if (!ctx) throw new Error(`未找到‘canvas’, getContext 错误`);
      if (!ctx) return;
        switch (background.type) {
          case "color":
            ctx.fillStyle = Array.isArray(background.color)
              ? background.color[
                  Math.ceil(Math.random() * (background.color.length - 1))
                ]
              : background.color;
            break;
          case "gradient":
            const gradient = ctx.createLinearGradient(
              position[0],
              position[1],
              position[2],
              position[3]
            );
            color.forEach((x, k) => {
              gradient.addColorStop(k / color.length, x);
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
      if (this.waves && this.waves.length) {
        loop();
        function loop() {
          if (clear) ctx.clearRect(0, 0, width, height);
          if(create) setColor(background);
          ctx.fillRect(0, 0, width, height);
          for (let i = 0; i < this.waves.length; i++) {
            let { jitter, restore, waveGap, waterGap, waveUps } = this.waves[i];
            ctx.beginPath();
            for (let e = 0; e <= 1 + 0.01; e += 0.01) {
              ctx.lineTo(
                e * width,
                height * this.waves[i]["waveHeight"] +
                  (Math.sin(e * waveUps + t * jitter) * waveGap + waterGap) *
                    Math.sin(t * restore)
              );
            }
            ctx.lineTo(width, height);
            ctx.lineTo(0, height);
            ctx.closePath();
            setColor(this.waves[i]["background"]);
            ctx.fill();
          }
          t++;
          requestAnimationFrame(loop);
        }
      }
    },
  },
};
</script>