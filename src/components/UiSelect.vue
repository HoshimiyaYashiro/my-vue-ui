<template name="ui-select">
  <div class="ui-select" v-click-outside="closeDropdown">
    <input
      type="text"
      class="ui-input-inner"
      placeholder="Select an option"
      readonly
      @click="openDropdown = !openDropdown"
      :value="optionSelected.name"
      :name="name"
      :disabled="disabled"
    />
    <i
      class="icon ion-ios-arrow-down"
      :class="{active: openDropdown, disabled: disabled}"
      @click="openDropdown = !openDropdown"
    ></i>
    <transition name="slide" v-on:enter="enter">
      <UiScroll :parent-class="'ui-dropdown-inner'" v-show="openDropdown" ref="scroll">
        <ul class="ui-dropdown-group">
          <li v-if="empty" @click="selectOption(emptyOption)" class="empty-option"></li>
          <li
            v-for="option in options"
            :key="option.value"
            @click="!option.disabled ? selectOption(option) : null"
            :class="{active: option.value === optionSelected.value, disabled: option.disabled}"
            ref="options"
          >
            <TextScroll :text="option.name"></TextScroll>
          </li>
        </ul>
      </UiScroll>
    </transition>
  </div>
</template>

<style lang="sass">
.ui-select
  display: inline-block
  width: 20rem
  position: relative
  .icon
    font-size: 1rem
    position: absolute
    right: 10px
    top: 1px
    line-height: 2.375rem
    cursor: pointer
    transition: all 0.2s ease
    color: lightgray
    &.active
      transform: rotate(-180deg)
    &.disabled
      cursor: not-allowed
      pointer-events: none
  .ui-input-inner
    width: calc(100% - 2.75rem)
    padding-right: 1.75rem
</style>

<script>
import UiScroll from './UiScroll';
import TextScroll from './TextScroll';

export default {
  name: 'UiSelect',
  props: ['options', 'name', 'value', 'empty', 'disabled'],
  data() {
    return {
      openDropdown: false,
      emptyOption: {
        name: "",
        value: null
      },
      optionSelected: null
    };
  },
  methods: {
    enter(el) {
      this.$refs.scroll.initScrollPercentage();
      if (this.optionSelected.value != null) {
        setTimeout(() => {
          this.$el.querySelector('li.active').scrollIntoView();
        }, 200);
      }
    },
    selectOption(option) {
      this.optionSelected = option;
      this.openDropdown = false;
      this.$emit('input', option.value);
    },
    closeDropdown(e) {
      this.openDropdown = false;
    }
  },
  components: {
    UiScroll,
    TextScroll
  },
  mounted() {
    if (this.value) {
      for (let option of this.options) {
        if (option.value === this.value) {
          this.optionSelected = option;
          break;
        }
      }
    } else {
      this.optionSelected = JSON.parse(JSON.stringify(this.emptyOption));
    }
  }
};
</script>