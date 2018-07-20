<template>
  <section class="bdso-table">
    <div class="form-group row">
      <label class="col-12">
        Table Section
      </label>
    </div>
    <div class="row">
      <label class="col-12">
        Table Heading
      </label>
    </div>
    <div class="form-group row">
      <div v-if="multipleRows" class="action-row">
      </div>
      <div class="col-1">
        <label class="table-heading position-absolute bottom-0">TH</label>
      </div>
      <div v-for="(column) in columns" :key="column.id" class="col pl-0">
        <a  v-if="multipleColumns" class="m-auto pl-2" @click="removeColumn(column.id)">
          <font-awesome-icon class="fa-lg text-danger primary" icon="minus-circle"/>
        </a>
        <input class="form-control" type="text" :placeholder="column.id">
      </div>
      <div @click="addColumn" class="col-1 add-column m-auto p-0" :class="{'add-disabled' : maxColumns}">
        <font-awesome-icon class="fa-lg" icon="plus-circle"/>
      </div>
    </div>
    <hr/>
    <div class="row">
      <label class="col-12">
        Table Rows
      </label>
    </div>
    <draggable v-model="rows" :options="{animation:200}" :no-transition-on-drag="true" @start="drag=true" @end="handleDrop">
      <transition-group :name="!drag ? 'list-complete' : null" :css="true">
        <div v-for="(row) in rows" :key="row.id" class="form-group row">
          <div v-if="multipleRows" class="m-auto pl-3 action-row">
            <a class="" @click="removeRow(row.id)">
              <font-awesome-icon class="fa-lg text-danger primary" icon="minus-circle"/>
            </a>
          </div>
          <div class="col-1">
            <input readonly class="form-control" :placeholder="row.id">
          </div>
          <div class="col pl-0" v-for="(column) in columns" :key="column.id" >
            <input class="form-control" type="text" :placeholder="column.text">
          </div>
          <div class="col-1 m-auto p-0 bars-container">
            <font-awesome-icon v-if="multipleRows" icon="bars" class="bars fa-2x"/>
          </div>
        </div>
      </transition-group>
    </draggable>
    <hr/>
    <div class="col-2">
      <a @click="addRow()">
        <font-awesome-icon class="text-success" icon="plus-circle"/> Add Row
      </a>
    </div>
  </section>
</template>
<script>
import draggable from 'vuedraggable'
import { store } from '../store/Store'
export default {
  name: 'bdso-table',
  components: {
    draggable
  },
  data () {
    return {
      drag: null,
      rows: [
        {
          id: Math.floor(Math.random() * Math.floor(1000)),
          row: ''
        }
      ],
      columns: [
        {
          id: 0,
          column: 'column'
        }
      ]
    }
  },
  computed: {
    multipleRows () {
      return this.rows.length > 1
    },
    multipleColumns () {
      return this.columns.length > 1
    },
    maxColumns () {
      return this.columns.length > 3
    }
  },
  methods: {
    addRow () {
      try {
        this.rows.push({
          id: Math.floor(Math.random() * Math.floor(1000)),
          row: ''})
      } catch (e) {
        console.log(e)
      }
    },
    removeRow (id) {
      let deletedId = id
      this.rows = this.rows.filter((row) => {
        return row.id !== deletedId
      })
    },
    addColumn () {
      if (this.maxColumns) return
      try {
        this.columns.push({
          id: Math.floor(Math.random() * Math.floor(1000)),
          column: 'anotherColumn'})
      } catch (e) {
        console.log(e)
      }
    },
    removeColumn (id) {
      let deletedId = id
      this.columns = this.columns.filter((column) => {
        return column.id !== deletedId
      })
    },
    handleDrop () {
      store.dispatch('moduleStore/storeModules')
    }
  }
}
</script>
<style scoped lang="scss">
  .action-row {
    width: 35px;
  }

  .add-column {
    svg {
      color: green;
    }

    &.add-disabled {
      color: lightgray;
      cursor: not-allowed;
      svg {
        color: lightgray;
      }
    }
  }

  .table-heading {
    bottom: 0;
  }
</style>
