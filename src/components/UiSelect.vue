<template name="ui-select">
  <div class="ui-select">
    <input type="text" class="ui-input-inner" placeholder="Select an option" readonly @click="openDropdown = !openDropdown"/>
    <i class="icon ion-ios-arrow-down" :class="{active: openDropdown}" @click="openDropdown = !openDropdown"></i>
    <transition name="slide" v-on:enter="enter">
      <UiScroll :parent-class="'ui-dropdown-inner'" v-show="openDropdown" ref="scroll">
        <ul class="ui-dropdown-group">
          <li v-for="option in options" :key="option.value"><TextScroll :text="option.name"></TextScroll></li>
        </ul>
      </UiScroll>
    </transition>
  </div>
</template>

<script>
import UiScroll from './UiScroll';
import TextScroll from './TextScroll';

export default {
  name: "UiSelect",
  props: ["options", "result"],
  data() {
    return {
      openDropdown: false
    };
  },
  methods: {
    enter: function (el) {
      this.$refs.scroll.initScrollPercentage();
    }
  },
  components: {
    UiScroll,
    TextScroll
  },
  mounted() {}
};
</script>