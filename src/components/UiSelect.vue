<template name="ui-select">
  <div class="ui-select">
    <input type="text" class="ui-input-inner" placeholder="Select an option" readonly @click="openDropdown = !openDropdown"/>
    <transition name="slide" :after-enter="afterEnter">
      <UiScroll :parent-class="'ui-dropdown-inner'" v-if="openDropdown">
        <ul class="ui-dropdown-group">
          <li v-for="option in options" :key="option.value">{{option.name}}</li>
        </ul>
      </UiScroll>
    </transition>
  </div>
</template>

<script>
import UiScroll from "./UiScroll";

export default {
  name: "UiSelect",
  props: ["options", "result"],
  data() {
    return {
      openDropdown: false
    };
  },
  methods: {
    afterEnter: function (el) {
      console.log(this);
    }
  },
  components: {
    UiScroll
  },
  mounted() {}
};
</script>