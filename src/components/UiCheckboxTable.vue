<template name="ui-checkbox-table">
  <div class="ui-checkbox-table">
    <div class="table-header-wrap">
      <table class="table-header">
        <colgroup>
          <col v-for="col in checkboxCols" :key="col.id" :style="{width: col.style.width}" />
        </colgroup>
        <thead>
          <tr>
            <th v-for="col in checkboxCols" :key="col.id">{{col.name}}</th>
          </tr>
          <tr v-if="multiple">
            <th v-for="col in checkboxCols" :key="col.id">
              <label class="ui-checkbox-lbl" v-if="col.value != null">
                <input type="checkbox" v-model="cTable.cols[col.id]" />
                <span></span>
              </label>
            </th>
          </tr>
        </thead>
      </table>
    </div>
    <div class="table-row-wrap">
      <table class="table-group" v-for="group in checkboxGroups" :key="group.id">
        <colgroup>
          <col v-for="col in checkboxCols" :key="col.id" :style="{width: col.style.width}" />
        </colgroup>
        <thead :style="{'background-color': group.style.bgHeader}">
          <tr class="table-row">
            <th v-for="(col, index) in checkboxCols" :key="col.id" :class="{'cell-vertical-middle': index === 0}">
              <label class="ui-checkbox-lbl">
                <input type="checkbox" v-model="cTable.groups[col.id][group.id]" @change="checkRowGroup(cTable.groups[col.id][group.id], group, index)"/>
                <span></span>
              </label>
              <span v-if="index === 0">{{group.name}}</span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr class="table-row" v-for="row in group.checkboxChilds" :key="row.id">
            <td v-for="(col, index) in checkboxCols" :key="col.id" :class="{'cell-vertical-middle': index === 0}">
              <label class="ui-checkbox-lbl">
                <input type="checkbox" v-model="cTable.cells[col.id][group.id][row.id]"/>
                <span></span>
              </label>
              <span v-if="index === 0">{{row.name}}</span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style lang="sass">
.ui-checkbox-table
  display: inline-block
  width: 700px
  table
    width: 100%
    .ui-checkbox-lbl
      margin: 0 auto
.ui-checkbox-lbl
    width: 13px
    height: 13px
    background: #fff
    position: relative
    cursor: pointer
    transition: all 0.3s ease
    margin-bottom: 0 !important
    border: solid 1px #d9d9d9
    display: block
    input[type=checkbox]
      display: none
      &~span
        display: none
      &:checked
        &~span
          display: block
          width: 100%
          height: 100%
          position: relative
          &::before
            content: ''
            display: inline-block
            left: 3px
            bottom: 1px
            width: 6px
            height: 12px
            border: 1px solid #007CA4
            border-width: 0 2px 2px 0
            transform: rotate(30deg)
            position: absolute
.ui-checkbox-table
  .table-row
    height: 1.875rem
    .cell-vertical-middle
      text-align: left
      > *
        vertical-align: middle
        display: inline-block
      > .ui-checkbox-lbl
        margin: 0 10px
      > span
        max-width: calc(100% - 40px)
        word-break: break-all
</style>
<script>
export default {
  props: ['checkboxCols', 'multiple', 'checkboxGroups'],
  data() {
    return {
      cTable: {
        cols: {},
        groups: {},
        cells: {}
      }
    };
  },
  methods: {
    checkRowGroup(value, group, colIndex) {
      const checkboxByGroups = this.cTable.groups;
      if (colIndex === 0) {
        for (let j = 1; j < this.checkboxCols.length; j++) {
          checkboxByGroups[this.checkboxCols[j].id][group.id] = value;
        }
      } else {
        let checkAll = true;
        for (let j = 1; j < this.checkboxCols.length; j++) {
          if (!this.cTable.groups[this.checkboxCols[j].id][group.id]) {
            checkAll = false; 
            break;
          }
        }
        checkboxByGroups[this.checkboxCols[0].id][group.id] = checkAll;
      }
    }
  },
  beforeMount() {
    var self = this;
    (this.multiple === this.multiple) !== null ? this.multiple : false;
    if (this.checkboxCols == null || !Array.isArray(this.checkboxCols))
      this.checkboxCols = [];
    if (this.checkboxGroups == null || !Array.isArray(this.checkboxGroups))
      this.checkboxGroups = [];
    this.checkboxGroups.forEach(group => {
      if (group.style == null) group.style = {bgHeader: '#f2f2f2'};
    });
    const checkboxByGroups = {};
    const checkboxByCells = {};
    this.checkboxCols.forEach(col => {
      self.cTable.cols[col.id] = col.value;
      checkboxByGroups[col.id] = {};
      checkboxByCells[col.id] = {};
      self.checkboxGroups.forEach(group => {
        checkboxByGroups[col.id][group.id] = col.value != null ? col.value : false;
        checkboxByCells[col.id][group.id] = {};
        group.checkboxChilds.forEach(c => {
          checkboxByCells[col.id][group.id][c.id] = col.value != null ? col.value : false;
        });
      });
    });
    this.cTable.groups = checkboxByGroups;
    this.cTable.cells = checkboxByCells;
  },
  mounted() {}
};
</script>
