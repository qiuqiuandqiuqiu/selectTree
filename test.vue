<template>
  <div>
    <el-select v-model="form" placeholder="请选择" multiple collapse-tags clearable @change="translate">
      <el-option
        :value="value"
        style="background:white;overflow:scroll;height:150px;overflow-x:hidden;"
      >
        <div>
          <el-tree
            :data="childData"
            show-checkbox
            :props="defaultProps"
            ref="child"
            :node-key="code"
            :default-expand-all="true"
          ></el-tree>
        </div>
      </el-option>
      <div style="display:flex;justify-content:center;align-items:center;margin-top:5%;">
        <el-button @click="sure">确定</el-button>
        <el-button @click="all">全选</el-button>
        <el-button @click="clear">清空</el-button>
      </div>
    </el-select>
  </div>
</template>
<script>
//childData为传入的数组
//name为下拉框上面多选的栏目的名字
//code为栏目名字中的编码
//chidldName为传入的子节点的名称
export default {
  props:{
        childData:Array,
        name:String,
        code:String,
        childrenName:String
    },
  data() {
    return {
      value: "",
      defaultProps: {
        children: this.childrenName,
        label: this.name
      },
      form: [],
      codeForm:[],
      isAll:false,
      x:true,
      list:[]
    };
  },
  methods: {
    //确定，并返回一个数组，里面是前面传入的遍历的code
    sure() {
      let list = this.$refs.child.getCheckedNodes();
      this.form = [];
      this.codeForm = [];
      for (let a in list) {
        this.form.push(eval("list[a]"+'.'+this.name));
        this.codeForm.push(eval("list[a]"+'.'+this.code));
      }
      this.$emit('data',this.codeForm)
      this.x=true
    },
    //清空
    clear() {
      this.form = [];
      this.$refs.child.setCheckedKeys([]);
    },  
    //全选
    all(){
      if(this.isAll==false){
        this.$refs.child.setCheckedNodes(this.childData);
        this.isAll=true
      }else{
        this.$refs.child.setCheckedKeys([]);
        this.isAll=false
      }
    },
    //下拉框选择到的数据进行删除后，树前面的勾选取消操作
    translate(){
      let list2=[];
      if(this.x==true){
        this.list=this.$refs.child.getCheckedNodes();
        for(let i=1;i<this.list.length;i++){
          list2.push(this.list[i])
        }
        this.list=list2
        this.x=false;
      }else{
        for(let i=1;i<this.list.length;i++){
          list2.push(this.list[i])
        }
        this.list=list2
        
      }
      this.$refs.child.setCheckedNodes(list2);
      this.codeForm=[]
      for(let a in list2){
        this.codeForm.push(eval("list2[a]"+'.'+this.code));
      }
      this.$emit('data',this.codeForm)
    }
  }
};
</script>