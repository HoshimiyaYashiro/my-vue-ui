<template name="ui-select">
  <div class="ui-select">
    <input type="text" class="ui-input-inner" placeholder="Select an option" readonly />
    <UiScroll :parent-class="'ui-dropdown-inner'">
      <ul class="ui-dropdown-group">
        <li v-for="option in options" :key="option.value">{{option.name}}</li>
      </ul>
    </UiScroll>
  </div>
</template>

<script>
import UiScroll from "./UiScroll";

export default {
  name: "UiSelect",
  props: ["options", "result"],
  data() {
    return {};
  },
  methods: {},
  components: {
    UiScroll
  },
  mounted() {}
};
</script>