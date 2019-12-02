<template name="ui-select">
  <div class="ui-select">
    <input type="text" class="ui-input-inner" placeholder="Select an option" readonly @click="openDropdown = !openDropdown" :value="optionSelected.name" :name="name"/>
    <i class="icon ion-ios-arrow-down" :class="{active: openDropdown}" @click="openDropdown = !openDropdown"></i>
    <transition name="slide" v-on:enter="enter">
      <UiScroll :parent-class="'ui-dropdown-inner'" v-show="openDropdown" ref="scroll">
        <ul class="ui-dropdown-group">
          <li v-for="option in options" :key="option.value" @click="selectOption(option)" :class="{active: option.value === optionSelected.value}" ref="options"><TextScroll :text="option.name"></TextScroll></li>
        </ul>
      </UiScroll>
    </transition>
  </div>
</template>

<script>
import UiScroll from './UiScroll';
import TextScroll from './TextScroll';

export default {
  name: 'UiSelect',
  props: ['options', 'name', 'value'],
  data() {
    return {
      openDropdown: false,
      optionSelected: {
        name: '',
        value: null
      }
    };
  },
  methods: {
    enter(el) {
      this.$refs.scroll.initScrollPercentage();
      if (this.optionSelected.value != null) {
        this.$el.querySelector('li.active').scrollIntoView();
      }
    },
    selectOption(option) {
      this.optionSelected = option;
      this.openDropdown = false;
      this.$emit('input', option.value);
    }
  },
  components: {
    UiScroll,
    TextScroll
  },
  mounted() {
    for (let option of this.options) {
      if (option.value === this.value) {
        this.optionSelected = option;
        break;
      }
    }
  }
};
</script>