<template>
  <div style="width: 100%; overflow: auto">
    <bk-input
      v-model="searchVal"
      placeholder="Search"
      type="search"
    />
    <div class="row">
      <div class="cell">
        <span>简化配置: <code>search = string</code></span>
        <bk-tree
          :data="treeData"
          :search="searchVal"
          children="children"
          label="name"
        />
      </div>
      <div class="cell">
        <span>详细配置：<code>search = { SearchOption }</code></span>
        <bk-tree
          :data="treeData"
          :search="searchOption1"
          children="children"
          label="name"
        />
      </div>
      <div class="cell">
        <span>详细配置-自定义搜索条件：<code>search = { SearchOption }</code></span>
        <bk-tree
          :data="treeData"
          :search="searchOption2"
          children="children"
          label="name"
        />
      </div>
    </div>
  </div>
</template>

<script>
  import { defineComponent } from 'vue';

  import { BASIC_DATA } from './options';
  export default defineComponent({
    components: {},
    data() {
      return {
        treeData: [...BASIC_DATA],
        searchVal: '',
      };
    },
    computed: {
      searchOption1() {
        return {
          /**
           * 需要匹配的值
           * 支持字符串
           * */
          value: this.searchVal,

          /**
           * 支持模糊匹配（fuzzy） || 完全匹配（full）
           * 默认 模糊匹配（fuzzy）
           * 支持自定义匹配函数 (searchValue, itemText, item) => true || false
           */
          match: 'fuzzy',

          /**
           * 显示为 tree || list
           * 默认 tree
           */
          resultType: 'tree',
        };
      },
      searchOption2() {
        return {
          /**
           * 需要匹配的值
           * */
          value: this.searchVal,

          /**
           * 支持模糊匹配（fuzzy） || 完全匹配（full）
           * 默认 模糊匹配（fuzzy）
           * 支持自定义匹配函数 (searchValue, itemText, item) => true || false
           */
          match: this.searchFn,

          /**
           * 显示为 tree || list
           * 默认 tree
           */
          resultType: 'tree',
          showChildNodes: false,
        };
      },
    },
    methods: {
      searchFn(searchValue, itemvalue) {
        const match = new RegExp(searchValue, 'i').test(itemvalue);
        return match;
      },
    },
  });
</script>
<style scoped>
  @import './tree.less';
</style>
