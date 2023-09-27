<template>
    <div>
      <el-tree :data="menu" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
    </div>
</template>

<script>
export default { //导出控件,需用data(),否则会编程单例模式
    data() {
      return {
        menu: [],
        defaultProps: {
          children: 'children',
          label: 'label'
        }
      };
    },
    methods: {
      handleNodeClick(data) {
        console.log(data);
      },

      getMenu(){
        this.$http({
          url: this.$http.adornUrl('/product/category/list/tree'),
          method: 'get',
          params: this.$http.adornParams({
            // 'page': this.pageIndex,
            // 'limit': this.pageSize,
            // 'roleName': this.dataForm.roleName
          })
        }).then(({data}) => {
            console.log("成功获取数据....",data)
        })
      }
    },
    created() {
      this.getMenu();
    }
};
</script>

<style>

</style>
