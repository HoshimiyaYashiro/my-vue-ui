<template>
  <span class="pre-text-scroll" :class="{'text-scroll': isScroll}" :style="{'animation-duration': duration}">
    {{text}}
  </span>
</template>

<script>
export default {
  props: ["text"],
  data() {
    return {
      isScroll: false,
      duration: 0
    };
  },
  methods: {
    scrollText(e) {
      e.stopPropagation();
      const $parent = this.$el.parentNode;
      if (this.$el.offsetWidth > $parent.offsetWidth) {
        this.isScroll = true;
        this.duration = (this.$el.offsetWidth / $parent.offsetWidth) * ($parent.offsetWidth / 80) + 's';
      }
    },
    stopScroll() {
      this.isScroll = false;
    }
  },
  mounted() {
    this.$el.parentNode.addEventListener('mouseenter', this.scrollText);
    this.$el.parentNode.addEventListener('mouseleave', this.stopScroll);
  },
};
</script>