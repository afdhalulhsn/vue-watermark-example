<template>
  <div class="watermark-container">
    <div
      class="watermark"
      v-for="(position, index) in watermarkPositions"
      :key="index"
      :style="{ top: position.top, left: position.left }"
    >
      AFDHAL GANTENG
    </div>
  </div>
</template>

<script>
export default {
  name: 'PageWatermark',
  data() {
    return {
      watermarkPositions: []
    };
  },
  mounted() {
    this.calculateWatermarkPositions();
    window.addEventListener('resize', this.calculateWatermarkPositions);
  },
  unmounted() {
    window.removeEventListener('resize', this.calculateWatermarkPositions);
  },
  methods: {
    calculateWatermarkPositions() {
      const watermarkWidth = 50; // Width of the watermark (adjust as needed)
      const watermarkHeight = 15; // Height of the watermark (adjust as needed)
      const spacing = 50; // Spacing between each watermark (adjust as needed)
      const windowHeight = window.innerHeight;
      const windowWidth = window.innerWidth;
      const positions = [];

      // Calculate diagonal positions
      for (let top = -watermarkHeight; top < windowHeight; top += watermarkHeight + spacing) {
        for (let left = -watermarkWidth; left < windowWidth; left += watermarkWidth + spacing) {
          positions.push({ top: `${top}px`, left: `${left}px` });
        }
      }

      this.watermarkPositions = positions;
    },
    detectScreenshot() {
      const sensitiveElement = document.querySelector('.sensitive-info');

      // Listen for Print Screen key
      document.addEventListener('keyup', event => {
        if (event.key === 'PrintScreen') {
          this.hideSensitiveInfo(sensitiveElement);
        }
      });

      // Listen for screenshot event (experimental, may not work in all browsers)
      window.addEventListener('screenshotTaken', () => {
        this.hideSensitiveInfo(sensitiveElement);
      });
    },
    hideSensitiveInfo(element) {
      // Replace sensitive information with asterisks or hide the element
      element.innerText = '*******'; // Example: Replace text with asterisks

      // Optionally, you can hide the element entirely:
      // element.style.display = 'none';
    }
  }
};
</script>

<style scoped>
.watermark-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
  pointer-events: none;
}

.watermark {
  font-size: 12pt; /* Font size of the watermark text */
  color: rgba(0, 0, 0, 0.2); /* Color and opacity of the watermark */
  position: absolute;
  transform: rotate(-45deg); /* Rotate the watermark diagonally */
  transform-origin: center center; /* Center rotation point */
}
</style>
