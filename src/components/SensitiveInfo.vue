<template>
  <div class="sensitive-info">
    <p v-if="!showMaskedInfo">
      Sensitive Information: {{ sensitiveInfo }}
    </p>
    <p v-else>
      Sensitive Information: *******
    </p>
  </div>
</template>

<script>
let isScreenshotDetected = false;
let timeoutId = null;

export default {
  name: 'SensitiveInfo',
  data() {
    return {
      sensitiveInfo: 'Your sensitive information', // Replace with actual sensitive data
      showMaskedInfo: false,
    };
  },
  mounted() {
    this.detectScreenshot();
  },
  methods: {
    detectScreenshot() {
      // Listen for Print Screen key
      document.addEventListener('keyup', event => {
        if (event.key === 'PrintScreen') {
          this.handleScreenshotAttempt();
        }
      });

      // Listen for screenshot event (experimental, may not work in all browsers)
      window.addEventListener('screenshotTaken', () => {
        this.handleScreenshotAttempt();
      });

      // Check periodically for changes indicative of a screenshot attempt
      setInterval(() => {
        if (this.isDOMChanged()) {
          this.handleScreenshotAttempt();
        }
      }, 1000); // Adjust interval as needed
    },
    isDOMChanged() {
      // Implement logic to check for rapid changes in DOM
      // Example: Compare current DOM state with previous state
      return false; // Placeholder, implement your logic here
    },
    handleScreenshotAttempt() {
      if (!isScreenshotDetected) {
        isScreenshotDetected = true;
        this.showMaskedInfo = true; // Mask sensitive information
        timeoutId = setTimeout(() => {
          this.showMaskedInfo = false; // Restore sensitive information
          isScreenshotDetected = false;
        }, 2000); // Restore after 2 seconds, adjust as needed
      }
    },
    clearTimer() {
      if (timeoutId) {
        clearTimeout(timeoutId);
        this.showMaskedInfo = false; // Restore sensitive information immediately
        isScreenshotDetected = false;
      }
    }
  },
  unmounted() {
    // Cleanup event listeners when component is destroyed
    document.removeEventListener('keyup', this.handleScreenshotAttempt);
    window.removeEventListener('screenshotTaken', this.handleScreenshotAttempt);
  }
};
</script>

<style scoped>
/* Add component-specific styles if needed */
</style>
