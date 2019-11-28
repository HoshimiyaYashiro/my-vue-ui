<template name="ui-scroll">
  <div class="ui-scroll-root" :class="parentClass">
    <div class="ui-scroll-wrap" @scroll="scrollHandle($event)" ref="scrollWrap">
      <slot></slot>
    </div>
    <div class="ui-scrollbar-bar is-vertical" ref="verticalScroll" :class="{active: isMouseDown}" @click.self.prevent="clickTrackHandle($event, 'vertical')">
      <div class="ui-scrollbar-thump"
        :style="{height: vertical.percentage + '%', transform: 'translateY(' + vertical.move + '%)'}"
        @mousedown="mouseDownHandle($event, 'vertical')" ref="verticalThump"
      ></div>
    </div>
    <div class="ui-scrollbar-bar is-horizontal" ref="horizontalScroll" @click.self.prevent="clickTrackHandle($event, 'horizontal')">
      <div class="ui-scrollbar-thump" 
      :style="{width: horizontal.percentage + '%', transform: 'translateX(' + horizontal.move + '%)'}" 
      @mousedown="mouseDownHandle($event, 'horizontal')" ref="horizontalThump"></div>
    </div>
  </div>
</template>
,
<script>
export default {
    name: 'UiScroll',
    props: ['parentClass'],
    components: {},
    data() {
        return {
          vertical: {
            percentage: 0,
            move: 0,
            scrollByPx: 0,
            point: 0,
            offset: 'offsetHeight',
            client: 'clientY',
            direction: 'top',
            scroll: 'scrollTop',
            scrollSize: 'scrollHeight',
            thump: 'verticalThump',
            track: 'verticalScroll'
          },
          horizontal: {
            percentage: 0,
            move: 0,
            scrollByPx: 0,
            point: 0,
            offset: 'offsetWidth',
            client: 'clientX',
            direction: 'left',
            scroll: 'scrollLeft',
            scrollSize: 'scrollWidth',
            thump: 'horizontalThump',
            track: 'horizontalScroll'
          },
          isMouseDown : false,
          bar: null
        }
    },
    methods: {
        scrollHandle() {
            const $el = this.$refs.scrollWrap;
            if (this.horizontal.scrollByPx != $el.scrollLeft) {
                this.horizontal.scrollByPx = $el.scrollLeft;
                this.horizontal.move = Math.round(($el.scrollLeft / $el.clientWidth * 100) * 1000) / 1000;
            }
            if (this.vertical.scrollByPx != $el.scrollTop) {
                this.vertical.scrollByPx = $el.scrollTop;
                this.vertical.move = Math.round(($el.scrollTop / $el.clientHeight * 100) * 1000) / 1000;
            }
        },
        mouseMoveHandle(e) {
          if (!this.isMouseDown) return;
          if (!this.bar.point) return;
          const $thump = this.$refs[this.bar.thump];
          const $track = this.$refs[this.bar.track];
          const $wrap = this.$refs.scrollWrap;
          const offset = (($track.getBoundingClientRect()[this.bar.direction] - e[this.bar.client]) * -1);
          const thumbClickPosition = ($thump[this.bar.offset] - this.bar.point);
          const thumbPositionPercentage = ((offset - thumbClickPosition) * 100 / $track[this.bar.offset]);
          const scroll = Math.round((thumbPositionPercentage * $wrap[this.bar.scrollSize] / 100) * 1000) / 1000;
          $wrap[this.bar.scroll] = scroll;
        },
        mouseUpHandle(e) {
          this.isMouseDown = false;
          this.bar.point = 0;
          document.removeEventListener('mousemove', this.mouseMoveHandle);
          document.onselectstart = null;
        },
        mouseDownHandle(e, bar) {
          if (e.ctrlKey || e.button === 2) return;
          this.bar = this[bar];
          this.isMouseDown = true;
          document.addEventListener('mousemove', this.mouseMoveHandle);
          document.addEventListener('mouseup', this.mouseUpHandle);
          document.onselectstart = () => false;
          this.bar.point = (e.currentTarget[this.bar.offset] - (e[this.bar.client] - e.currentTarget.getBoundingClientRect()[this.bar.direction]));
        },
        clickTrackHandle(e, bar) {
          this.bar = this[bar];
          const $wrap = this.$refs.scrollWrap;
          const $thump = this.$refs[this.bar.thump];
          const offset = Math.abs(e.target.getBoundingClientRect()[this.bar.direction] - e[this.bar.client]);
          const thumbHalf = ($thump[this.bar.offset] / 2);
          const thumbPositionPercentage = ((offset - thumbHalf) * 100 / e.target[this.bar.offset]);
          $wrap[this.bar.scroll] = (thumbPositionPercentage * $wrap[this.bar.scrollSize] / 100);
        },
        initScrollPercentage() {
          const $el = this.$refs.scrollWrap;
          this.vertical.percentage = Math.round(($el.clientHeight / $el.scrollHeight * 100) * 1000) / 1000;
          this.horizontal.percentage = Math.round(($el.clientWidth / $el.scrollWidth * 100) * 1000) / 1000;
        }
    },
    mounted() {
      const self = this;
      self.initScrollPercentage();
    },
    destroyed() {
      document.removeEventListener('mouseup', this.mouseUpHandle);
    }
}
</script>