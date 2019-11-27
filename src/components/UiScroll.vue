<template name="ui-scroll">
  <div class="ui-scroll-root" :class="parentClass">
    <div class="ui-scroll-wrap" @scroll="scrollHandle($event)" ref="scrollWrap">
      <slot></slot>
    </div>
    <div class="ui-scrollbar-bar is-vertical" ref="verticalScroll" :class="{active: isMouseDown}">
      <div
        class="ui-scrollbar-thump"
        :style="{height: scrollYPercentage + '%', transform: 'translateY(' + translateY + '%)'}"
        @mousedown="mouseDownHandle"
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
    components: {},
    data() {
        return {
            scrollYPercentage: 0,
            scrollXPercentage: 0,
            translateY: 0,
            translateX: 0,
            scrollY: 0,
            scrollX: 0,
            isMouseDown : false,
            axisY: 0
        }
    },
    methods: {
        scrollHandle() {
            const $el = this.$refs.scrollWrap;
            if (this.scrollX != $el.scrollLeft) {
                this.scrollX = $el.scrollLeft;
                this.translateX = Math.round(($el.scrollLeft / $el.clientWidth * 100) * 1000) / 1000;
            }
            if (this.scrollY != $el.scrollTop) {
                this.scrollY = $el.scrollLeft;
                this.translateY = Math.round(($el.scrollTop / $el.clientHeight * 100) * 1000) / 1000;
                console.log($el.scrollTop);
            }
        },
        mouseMoveHandle(e) {
          if (this.isMouseDown) {
            const $el = this.$refs.scrollWrap;
            const offset = ((this.$refs.verticalScroll.getBoundingClientRect().top - e.clientY) * -1);
            const thumbClickPosition = (e.target.offsetHeight - this.axisY);
            const thumbPositionPercentage = ((offset - thumbClickPosition) * 100 / this.$el.offsetHeight);
            const scrollTop = Math.round((thumbPositionPercentage * this.scrollYPercentage / 100) * 1000) / 1000;
            this.$refs.scrollWrap.scrollTop = scrollTop;
            console.log(scrollTop);
          }
        },
        mouseUpHandle(e) {
          this.isMouseDown = false;
          this.axisY = 0;
        },
        mouseDownHandle(e) {
          e.stopImmediatePropagation();
          this.isMouseDown = true;
          this.axisY = (e.currentTarget.offsetHeight - (e.clientY - e.currentTarget.getBoundingClientRect().top));
          document.addEventListener('mousemove', this.mouseMoveHandle);
          document.addEventListener('mouseup', this.mouseUpHandle);
          document.onselectstart = () => false;
        }
    },
    mounted() {
        const self = this;
        const $el = self.$refs.scrollWrap;
        self.scrollYPercentage = Math.round(($el.clientHeight / $el.scrollHeight * 100) * 1000) / 1000;
        self.scrollXPercentage = Math.round(($el.clientWidth / $el.scrollWidth * 100) * 1000) / 1000;
    },
    destroyed() {
      document.removeEventListener('mouseup', this.mouseUpHandle);
    }
}
</script>