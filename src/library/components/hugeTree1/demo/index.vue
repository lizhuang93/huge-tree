<template>
  <div class="huge-demo">
    <router-link :to="'/demo/hugeTree2'">hugeTree2</router-link>
    <button @click="nav">hugeTree2</button>
    <div class="btn-bar">
      huge-tree<br />
      点击按钮，展示tree<br />
      <button @click="btnClick('tree-500')">500条</button>
      <button @click="btnClick('tree-10000')">1w 条</button>
      <button @click="btnClick('tree-20000')">2w 条</button>
      <button @click="btnClick('tree-50000')">5w 条</button>
      <button @click="btnClick('tree-100000')">10w 条</button>
      <button @click="btnClick('tree-200000')">20w 条</button><br />
      <button @click="onReload">刷新</button>
    </div>

    <div class="tree-wrap" v-show="isShowDialog">
      <huge-tree
        ref="huge-tree"
        hasInput
        checkedAction="dblclick"
        :expandKeys="expandKeys"
        expandLevel="all"
        :isLoading="isLoading"
        :showCheckbox="true"
        :defaultCheckedKeys="checkedKeys"
        @onChange="onChange"
        @onClickLabel="onClickLabel"
        @onClickCheckbox="onClickCheckbox"
      >
        <select slot="pre-input" class="pre-select">
          <option value="1">1111</option>
          <option value="2">2222</option>
        </select>
        <span slot-scope="{ slotScope }"><i>&#9733;</i> {{ slotScope.label }}</span>
        <i slot="loading">加载中...</i>
      </huge-tree>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { HugeTree } from 'huge-tree';
export default {
  components: {
    HugeTree,
  },
  props: {},
  data() {
    return {
      checkedKeys: ['1-3'],
      isLoading: true,
      isShowDialog: false,
      data: [],
      expandKeys: [],
      // data: [
      //    {
      //       checked: false,
      //       id: 1,
      //       indeterminate: false, // 节点的子树中是否是部分选中， 代码生成
      //       label: '节点text'
      //       parentId: 0, // 父节点 id
      //       isLeaf: false, // 叶子节点
      //       path: [0, 1], // 节点位置
      //       isExpand: false, // 展开， 代码生成
      //       hidden: false, // 隐藏， 代码生成
      //       disabled: false, // 禁用， 可选
      //       leafCount: 100,  // 子、孙元素的数量，代码计算
      //     }
      // ],
    };
  },

  computed: {},

  mounted() {
    this.btnClick('tree-200000');
  },

  methods: {
    nav() {
      this.$router.replace('/demo/hugeTree2');
    },
    btnClick(count) {
      this.isShowDialog = true;
      axios.get(`/static/json/${count}.json`).then(({ data }) => {
        this.expandKeys = ['8-1', '10-1', '1-1'];
        this.$refs['huge-tree'].setData(data);
        this.isLoading = false;
        setTimeout(() => {
          this.checkedKeys = ['1-5'];
          // this.$refs['huge-tree'].setCheckedKeys(['1-3', '1-5']);
        }, 1000);
      });
    },
    onChange({ checkedKeys, checkedNodes }) {
      console.log('onChange', checkedKeys, checkedNodes);
    },
    onClickLabel(node) {
      console.log('onClickLabel', node);
    },
    onClickCheckbox(node) {
      console.log('onClickCheckbox', node);
    },
    onReload() {
      window.location.reload();
    },
  },
};
</script>

<style scoped lang="less" style="text/less">
.huge-demo {
  text-align: left;
  .btn-bar {
    padding: 20px;
    button {
      cursor: pointer;
      padding: 5px 10px;
      color: #ffffff;
      background-color: #409eff;
      border-radius: 4px;
      &:hover {
        background-color: #228af1;
      }
    }
  }
  .tree-wrap {
    height: 600px;
    width: 400px;
    .pre-select {
      cursor: pointer;
      padding: 5px 10px;
    }
  }
}
</style>
