<template>
  <div class="about">
    <UiSelect :options="select.options" v-model="select.result" :name="select.name" />
    <UiCheckboxTable :multiple="true" :checkbox-cols="checkboxTable.checkboxCols" :checkbox-groups="checkboxTable.checkboxGroups"/>
  </div>
</template>

<style lang="sass">
@import 'src/assets/style/ui-style.sass'
</style>

<script>
import UiSelect from "@/components/UiSelect.vue";
import UiCheckboxTable from "@/components/UiCheckboxTable.vue";

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
      },
      checkboxTable: {
        checkboxCols: [
          {
            id: 'col-label',
            name: 'ANALYSES',
            value: undefined,
            style: {
              width: '300px'
            }
          }, {
            id: 'col-realize',
            name: 'RÉALISÉ',
            value: false,
            style: {
              width: '100px'
            }
          }, {
            id: 'col-contract',
            name: 'CONTRAT',
            value: false,
            style: {
              width: '100px'
            }
          }, {
            id: 'col-budget',
            name: 'BUDGET',
            value: false,
            style: {
              width: '100px'
            }
          }
        ],
        checkboxGroups: [
          {
            id: 'group-supplies',
            name: 'FOURNITURES',
            checkboxChilds: [
              {
                id: 'cell-consumption-pcs',
                name: 'Conso PCS (MWh)',
                field: 'code_conso_totale_pcs'
              }, {
                id: 'cell-consumption-pci',
                name: 'Conso PCI (MWh)',
                field: 'code_conso_totale_pci'
              }
            ],
            disableCheckboxByCol: ['col-contract', 'col-budget']
          }, {
            id: 'group-weather-condition',
            name: 'CONDITIONS METEO',
            checkboxChilds: []
          }, {
            id: 'group-energy-use',
            name: 'USAGES ENERGETIQUES',
            checkboxChilds: []
          }, {
            id: 'group-total',
            name: 'RESULTATS SUR LE PERIMETRE',
            checkboxChilds: []
          }
        ]
      }
    };
  },
  methods: {
  },
  components: {
    UiSelect,
    UiCheckboxTable
  }
};
</script>