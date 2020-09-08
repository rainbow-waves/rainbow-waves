<template>
  <div v-if="createCanvas" class="rainbow-waves">
    <canvas :id="elId"></canvas>
  </div>
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
      return this.config ? this.config.new || true : true;
    },
    elId() {
      return this.config ? this.config.el || "rainbow-waves" : "rainbow-waves";
    },
  },
  watch: {
    config: {
      handler() {
        this.drow();
      },
      deep: true,
    },
    waves: {
      handler() {
        this.drow();
      },
      deep: true,
    },
  },
  mounted() {
    this.drow();
  },
  methods: {
    drow() {
      // 初始化
      let t = 0;
      let waves = [];
      let canvas = null;
      let ctx = null;
      let width = 0;
      let height = 0;

      // 配置项
      const { new: create, el, width: xW, height: xH, clear, background } = {
        el: "rainbow-waves",
        clear: true,
        new: true,
        width: 1920,
        height: 1000,
        background: {
          type: "color",
          color: "#fff",
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
        canvas.width = width = xW;
        canvas.height = height = xH;
        setColor(background);
        ctx.fillRect(0, 0, width, height);
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
      if (this.waves && this.waves.length) {
        waves = [...this.waves];
        loop();
        function loop() {
          if (clear) ctx.clearRect(0, 0, width, height);
          if (create) setColor(background);
          ctx.fillRect(0, 0, width, height);
          for (let i = 0; i < waves.length; i++) {
            let { jitter, restore, waveGap, waterGap, waveUps } = waves[i];
            ctx.beginPath();
            for (let e = 0; e <= 1 + 0.01; e += 0.01) {
              ctx.lineTo(
                e * width,
                height * waves[i]["waveHeight"] +
                  (Math.sin(e * waveUps + t * jitter) * waveGap + waterGap) *
                    Math.sin(t * restore)
              );
            }
            ctx.lineTo(width, height);
            ctx.lineTo(0, height);
            ctx.closePath();
            setColor(waves[i]["background"]);
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