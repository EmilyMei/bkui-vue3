<template>
  <div style="width: 100%; ">
    <div style="display: flex; padding: 15px 0; align-items: center">
      <bk-checkbox-group v-model="border">
        <bk-checkbox label="none" />
        <bk-checkbox label="row" disabled />
        <bk-checkbox label="col" />
        <bk-checkbox label="outer" />
        <bk-checkbox label="horizontal" />
      </bk-checkbox-group>
      <bk-button style="margin-left: 20px" @click="() => (tableData = [])">
        清空表格数据
      </bk-button>
    </div>

  </div>
  <bk-table :border="border" :columns="columns" :data="tableData" />
</template>

<script>
import { defineComponent } from 'vue';

import { DATA_COLUMNS, DATA_TABLE } from './options';
export default defineComponent({
  components: {},
  data() {
    return {
      tableData: [...DATA_TABLE],
      columns: [...DATA_COLUMNS],
      border: ['row'],
    };
  },
});
</script>
