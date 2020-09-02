<template>
  <div class="rainbow-waves">
    <canvas :id="config.el"></canvas>
  </div>
</template>

<script>
export default {
  name: "rainbow-waves",
  props: {
    config: {
      type: Object,
      default() {
        return {
          el: "rainbow-waves",
          width: 1920,
          height: 1000,
          backgroundColor: "#fff",
          backgroundImage:{
            src:"",
            repetition:"",
          },
          waves: [
            {
              color: "blue",
              jitter: 0.04,
              restore: 0.03,
              waveGap: 80,
              waterGap: 20,
              waveUps: 6,
              waveHeight: 0.45,
            },
          ],
        };
      },
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
      immediate: true,
    },
  },
  mounted() {},
  beforeDestroy() {
    const { el, width, height } = this.config;
    let canvas = document.getElementById(el);
    let ctx = canvas.getContext("2d");
    ctx.rect(0, 0, width, height);
  },
  methods: {
    drow() {
      const { el, width, height, waves, backgroundColor,backgroundImage } = this.config;
      if (waves && waves.length) {
        let canvas = document.getElementById(el);
        let ctx = canvas.getContext("2d");
        canvas.width = width;
        canvas.height = height;
        let t = 0;
        loop();
        function loop() {
          ctx.clearRect(0, 0, width, height);
          if (backgroundImage && backgroundImage.src) {
            let repetition = backgroundImage.repetition || "repeat";
            let img = new Image();
            img.src = backgroundImage.src;
            ctx.fillStyle = ctx.createPattern(img, repetition);
          } else {
            ctx.fillStyle = backgroundColor;
          }
          ctx.fillRect(0, 0, width, height);
          for (let i = 0; i < waves.length; i++) {
            let color = waves[i]["color"];
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
            ctx.fillStyle = color;
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