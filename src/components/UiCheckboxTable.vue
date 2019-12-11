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
            <th v-for="(col, index) in checkboxCols" :key="col.id">
              <label class="ui-checkbox-lbl" v-if="col.value !== undefined">
                <input type="checkbox" v-model="cTable.cols[col.id]" @change="checkColHandle(col, index)"/>
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
              <label class="ui-checkbox-lbl" v-if="cTable.groups[col.id][group.id] !== undefined">
                <input type="checkbox" v-model="cTable.groups[col.id][group.id]" @change="checkGroupHandle(col, group, index)" :disabled="cTable.groups[col.id][group.id] === null"/>
                <span></span>
              </label>
              <span v-if="index === 0">{{group.name}}</span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr class="table-row" v-for="row in group.checkboxChilds" :key="row.id">
            <td v-for="(col, index) in checkboxCols" :key="col.id" :class="{'cell-vertical-middle': index === 0}">
              <label class="ui-checkbox-lbl" v-if="cTable.cells[col.id][group.id][row.id] !== undefined">
                <input type="checkbox" v-model="cTable.cells[col.id][group.id][row.id]" @change="checkCellHandle(col, group, row, index)" :disabled="cTable.cells[col.id][group.id][row.id] === null"/>
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
      &:disabled
        &~span
          display: block
          width: 100%
          height: 100%
          background-color: #d9d9d9
          cursor: 
      &:checked
        &~span
          display: block
          width: 100%
          height: 100%
          position: relative
          &:before
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
    checkColRow(col, colIndex) {
      const checkboxByCols = this.cTable.cols;
      if (colIndex === 0) {
        for (let j = 1; j < this.checkboxCols.length; j++) {
          if (checkboxByCols[this.checkboxCols[j].id] != null) checkboxByCols[this.checkboxCols[j].id] = checkboxByCols[col.id];
        }
      } else {
        let checkAll = true;
        for (let j = 1; j < this.checkboxCols.length; j++) {
          let o = checkboxByCols[this.checkboxCols[j].id];
          if (o != null && !o) {
            checkAll = false; 
            break;
          }
        }
        checkboxByCols[this.checkboxCols[0].id] = checkAll;
      }
    },
    checkColHandle(col, colIndex) {
      this.checkColRow(col, colIndex);
      for (let i = 0; i < this.checkboxGroups.length; i++) {
        let group = this.checkboxGroups[i];
        if (this.cTable.groups[col.id][group.id] != null) {
          this.cTable.groups[col.id][group.id] = this.cTable.cols[col.id];
          this.checkGroupRow(col, group, colIndex);
          this.checkGroupToChild(this.cTable.cols[col.id], col, group, colIndex);
        }       
      }
    },
    checkGroupHandle(col, group, colIndex) {
      const checkboxByGroups = this.cTable.groups;
      const value = checkboxByGroups[col.id][group.id];
      this.checkGroupRow(col, group, colIndex);
      this.checkGroupToParent(col, group, colIndex);
      this.checkGroupToChild(value, col, group, colIndex);
    },
    checkGroupToChild(value, col, group, colIndex) {
      for (let i = 0; i < group.checkboxChilds.length; i++) {
        if (this.cTable.cells[col.id][group.id][group.checkboxChilds[i].id] != null) {
          this.cTable.cells[col.id][group.id][group.checkboxChilds[i].id] = value;
          this.checkCell(col, group, group.checkboxChilds[i], colIndex);
        }
      }
    },
    checkGroupRow(col, group, colIndex) {
      const checkboxByGroups = this.cTable.groups;
      const value = checkboxByGroups[col.id][group.id];
      if (colIndex === 0) {
        for (let j = 1; j < this.checkboxCols.length; j++) {
          if (checkboxByGroups[this.checkboxCols[j].id][group.id] != null) checkboxByGroups[this.checkboxCols[j].id][group.id] = value;
        }
      } else {
        let checkAll = true;
        for (let j = 1; j < this.checkboxCols.length; j++) {
          let o = checkboxByGroups[this.checkboxCols[j].id][group.id];
          if (o != null && !o) {
            checkAll = false; 
            break;
          }
        }
        checkboxByGroups[this.checkboxCols[0].id][group.id] = checkAll;
      }
    },
    checkGroupToParent(col, group, colIndex) {
      let checkAllByCol = true;
      for (let i = 0; i < this.checkboxGroups.length; i++) {
        let o = this.cTable.groups[col.id][this.checkboxGroups[i].id];
        if (o != null && !o) {
          checkAllByCol = false;
          break;
        }
      }
      if (this.cTable.cols[col.id] != null) this.cTable.cols[col.id] = checkAllByCol;
      if (colIndex === 0) {
        for (let i = 1; i < this.checkboxCols.length; i++) {
          this.checkGroupToParent(this.checkboxCols[i], group, i);
        }
      } else {
        this.checkColRow(col, colIndex);
      }
    },
    checkCell(col, group, row, colIndex) {
      const checkboxByCells = this.cTable.cells;
      const value = checkboxByCells[col.id][group.id][row.id];
      if (colIndex === 0) {
        for (let j = 1; j < this.checkboxCols.length; j++) {
          if (checkboxByCells[this.checkboxCols[j].id][group.id][row.id] != null) checkboxByCells[this.checkboxCols[j].id][group.id][row.id] = value;
        }
      } else {
        let checkAll = true;
        for (let j = 1; j < this.checkboxCols.length; j++) {
          let o = checkboxByCells[this.checkboxCols[j].id][group.id][row.id];
          if (o != null && !o) {
            checkAll = false; 
            break;
          }
        }
        checkboxByCells[this.checkboxCols[0].id][group.id][row.id] = checkAll;
      }
    },
    checkCellToParent(col, group, row, colIndex) {
      const checkboxByCells = this.cTable.cells;
      const value = checkboxByCells[col.id][group.id][row.id];
      let checkAllByCol = true;
      for (let i = 0; i < group.checkboxChilds.length; i++) {
        let o = checkboxByCells[col.id][group.id][group.checkboxChilds[i].id];
        if (o != null && !o) {
          checkAllByCol = false;
          break;
        }
      }
      if (this.cTable.groups[col.id][group.id] != null) this.cTable.groups[col.id][group.id] = checkAllByCol;
    },
    checkCellHandle(col, group, row, colIndex) {
      this.checkCell(col, group, row, colIndex);
      this.checkCellToParent(col, group, row, colIndex);
      this.checkGroupRow(col, group, colIndex);
      this.checkGroupToParent(col, group, colIndex);
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
      if (group.disableCheckboxByCol == null) group.disableCheckboxByCol = [];
    });
    const checkboxByGroups = {};
    const checkboxByCells = {};
    this.checkboxCols.forEach(col => {
      self.cTable.cols[col.id] = col.value;
      checkboxByGroups[col.id] = {};
      checkboxByCells[col.id] = {};
      self.checkboxGroups.forEach(group => {
        checkboxByGroups[col.id][group.id] = col.value != null ? col.value : false;
        for (let i = 0; i < group.disableCheckboxByCol.length; i++) {
          if (group.disableCheckboxByCol[i] === col.id) {
            checkboxByGroups[col.id][group.id] = null;
            break;
          }
        }
        checkboxByCells[col.id][group.id] = {};
        group.checkboxChilds.forEach(c => {
          checkboxByCells[col.id][group.id][c.id] = checkboxByGroups[col.id][group.id];
        });
      });
    });
    this.cTable.groups = checkboxByGroups;
    this.cTable.cells = checkboxByCells;
  },
  mounted() {}
};
</script>
