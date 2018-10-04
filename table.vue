<template>
  <section class="table-example">
    <h2>Dynamic Vue Table</h2>
    <div class="row mt-2">
      <label class="col-12">
        Table Section
      </label>
    </div>
    <div class="form-group row">
      <div v-if="tableData.length > 1" class="action-row">
      </div>
      <div class="col">
        <input id="th-a" class="form-control" type="text">
      </div>
      <div class="col">
        <input id="th-b" class="form-control" type="text">
      </div>
      <div class="col">
        <input id="th-c" class="form-control" type="text">
      </div>
      <div class="col">
        <input id="th-d" class="form-control" type="text">
      </div>
      <div class="col-1 add-column m-auto p-0">
      </div>
    </div>
    <hr/>
    <div class="row">
      <label class="col-12">
        Table Rows
      </label>
    </div>
    <draggable v-model="tableData" :options="{animation:200}" :no-transition-on-drag="true" @start="drag=true">
      <transition-group :name="!drag ? 'list-complete' : null" :css="true">
        <div v-for="(row, index) in tableData" :key="index" class="form-group row">
          <div v-if="tableData.length > 1" class="m-auto pl-3 action-row">
            <a class="link" @click="removeRow(index)">
              <font-awesome-icon class="fa-md text-danger primary" icon="minus-circle"/>
            </a>
          </div>
          <div class="col" :id="'A' + row.id">
            <input class="form-control" type="text" :placeholder="'A' + index" v-model="tableData[index].columnA">
          </div>
          <div class="col" :id="'B' + row.id">
            <input class="form-control" type="text" :placeholder="'B' + index" v-model="tableData[index].columnB">
          </div>
          <div class="col" :id="'C' + row.id">
            <input class="form-control" type="text" :placeholder="'C' + index" v-model="tableData[index].columnC">
          </div>
          <div class="col" :id="'D' + row.id">
            <input class="form-control" type="text" :placeholder="'D' + index" v-model="tableData[index].columnD">
          </div>
          <div class="col-1 m-auto p-0 bars-container">
            <font-awesome-icon v-if="tableData.length > 1" icon="bars" class="bars fa-md"/>
          </div>
        </div>
      </transition-group>
    </draggable>
    <hr/>
    <a class="link" @click="addRow()">
      <font-awesome-icon class="text-success" icon="plus-circle"/>
      Add Row
    </a>
  </section>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  components: {
    draggable
  },
  data () {
    return {
      drag: null,
      tableData: [
        {
          columnA: 'a',
          columnB: 'b',
          columnC: 'c',
          columnD: 'd'
        },
        {
          columnA: 'a',
          columnB: 'b',
          columnC: 'c',
          columnD: 'd'
        },
        {
          columnA: 'a',
          columnB: 'b',
          columnC: 'c',
          columnD: 'd'
        }
      ]
    }
  },
  methods: {
    addRow () {
      this.tableData.push({
        columnA: '',
        columnB: '',
        columnC: '',
        columnD: ''
      })
    },
    removeRow (deletedIndex) {
      this.tableData = this.tableData.filter((row, index) => {
        return index !== deletedIndex
      })
    }
  }
}
</script>

<style scoped lang="scss">
.action-row {
  width: 25px;
}

.bars-container {
  cursor: move;
}

.add-column {
  svg {
    color: green;
  }
}

.link {
  cursor: pointer;
}
</style>
