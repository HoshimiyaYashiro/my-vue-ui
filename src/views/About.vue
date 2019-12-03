<template>
  <div class="about">
    <UiSelect :options="select.options" v-model="select.result" :name="select.name"/>
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
          { value: "0", name: "Fate Grand Order Saber Ruler Narmaya Kurumi Miku Archer Lancer", disabled: true},
          { value: "1", name: "Onmyoji" },
          { value: "2", name: "Sword Art Online" },
          { value: "3", name: "Date A Live" },
          { value: "4", name: "Code Gear" },
          { value: "5", name: "Guilty Crown" }
        ],
        name: 'test-select'
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