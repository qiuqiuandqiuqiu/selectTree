<template>
  <div>
    <selectTree :childData="father" :name="name" :code="code" :childrenName="childrenName" @data="getData" >
    </selectTree>
  </div>
</template>
<script>
import selectTree from "./test.vue";
export default {
  components: {
    selectTree
  },
  data() {
    return {
      father: [],
      name:'deptName',
      form:[],
      code:'deptCode',
      childrenName:"children"
    };
  },
  created() {
    that = this;
  },
  mounted() {},
  methods: {
    getData(val){
      that.form=val //获取到的前面传入的对象名为code的数据列表
    }
  }
};
var that;
</script>
<style>
</style>