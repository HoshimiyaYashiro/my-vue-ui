<template name="ui-scroll">
  <div class="ui-scroll-root" :class="parentClass">
    <div class="ui-scroll-wrap" @scroll="scrollHandle($event)" ref="scrollWrap">
      <slot></slot>
    </div>
    <div class="ui-scrollbar-bar is-vertical" ref="verticalScroll">
      <div
        class="ui-scrollbar-thump"
        :style="{height: scrollYPercentage + '%', transform: 'translateY(' + translateY + '%)'}"
      ></div>
    </div>
    <div class="ui-scrollbar-bar is-horizontal" ref="horizontalScroll">
      <div class="ui-scrollbar-thump"></div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'UiScroll',
    props: ['parentClass'],
    data() {
        return {
            scrollYPercentage: 0,
            scrollXPercentage: 0,
            translateY: 0,
            translateX: 0,
            scrollY: 0,
            scrollX: 0
        }
    },
    methods: {
        scrollHandle() {
            const $el = this.$refs.scrollWrap;
            if (this.scrollX != $el.scrollLeft) {
                this.scrollX = $el.scrollLeft;
                this.translateX = ($el.scrollLeft / $el.clientWidth * 100).toFixed(4);
            }
            if (this.scrollY != $el.scrollTop) {
                this.scrollY = $el.scrollLeft;
                this.translateY = ($el.scrollTop / $el.clientHeight * 100).toFixed(4);
            }
        }
    },
    mounted() {
        const self = this;
        const $el = self.$refs.scrollWrap;
        self.scrollYPercentage = ($el.clientHeight / $el.scrollHeight * 100).toFixed(4);
        self.scrollXPercentage = ($el.clientWidth / $el.scrollWidth * 100).toFixed(4);
    },

}
</script>