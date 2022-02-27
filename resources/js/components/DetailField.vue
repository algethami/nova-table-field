<template>
  <panel-item :field="field">
    <template slot="value">
      <Table :edit-mode="false" class="overflow-hidden" v-if="theData.length > 0">
        <div class="bg-white overflow-hidden">
          <TableRow :disabled="true" :key="index" :row="row" v-for="(row, index) in theData" />
          <div
            v-if="number_of_pages > 1 && current_page < number_of_pages"
            @click="more"
            class="p-3 cursor-pointer"
            style="text-align: center !important; background-color: #d1d7dd">
            المزيد
          </div>
        </div>
      </Table>
    </template>
  </panel-item>
</template>

<script>
import TableRow from './FormFields/TableRow';
import Table from './FormFields/Table';

export default {
  props: ['resource', 'resourceName', 'resourceId', 'field'],

  components: { Table, TableRow },

  data: () => ({
    theData: [],
    number_of_pages: 1,
    current_page: 1,
    original: [],
  }),

  created() {
    let valuesArray = Array.isArray(this.field.value) ? this.field.value : JSON.parse(this.field.value);
    if (!Array.isArray(valuesArray) || !valuesArray.length) valuesArray = [];

    this.original = valuesArray;
    this.number_of_pages = Math.ceil(valuesArray.length / 20);
    this.loadData();
  },

  methods: {
    loadData() {
      this.theData = this.original.slice(0, this.current_page * 20);
    },

    more() {
      this.current_page++;
      this.loadData();
    }
  }
};
</script>
