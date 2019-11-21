<template>
  <div class="about">
    <UiSelect :options="select.options" :result="select.result" />
  </div>
</template>

<style lang="sass">
@import 'src/assets/style/ui-style.sass'
</style>

<script>
import UiSelect from "@/components/UiSelect.vue";

export default {
  data() {
    return {
      select: {
        result: null,
        options: [
          { value: "0", name: "Fate Grand Order" },
          { value: "1", name: "Onmyoji" },
          { value: "2", name: "Sword Art Online" },
          { value: "3", name: "Date A Live" },
          { value: "4", name: "Code Gear" },
          { value: "5", name: "Guilty Crown" }
        ]
      }
    };
  },
  methods: {
  },
  components: {
    UiSelect
  }
};
</script>