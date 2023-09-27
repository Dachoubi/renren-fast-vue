<template>
    <div>
      <el-tree :data="menu" :props="defaultProps" @node-click="handleNodeClick" node-key="catId" :expand-on-click-node="false" show-checkbox :default-expanded-keys="expandedKey">
          <span class="custom-tree-node" slot-scope="{node, data}">
              <span>{{ node.label }}</span>
              <span>
                <el-button v-if="node.level <= 2" type="text" size="mini" @click="() => append(data)">&nbsp;新增</el-button>
                <el-button v-if="node.level === 3" type="text" size="mini" @click="() => remove(node, data)">删除</el-button>
              </span>
         </span>

      </el-tree>
    </div>
</template>

<script>
export default { //导出控件,需用data(),否则会编程单例模式
    data() {
      return {
        menu: [],
        expandedKey:[],
        defaultProps: {
          children: 'children',
          label: 'name'
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
            this.menu = data.data;
        })
      },
      append(data) {

      },
      remove(node, data) {

        console.log("node-",node);
        console.log("data-",data);
        console.log("node.parent.data.catId",node.parent.data.catId)
        console.log("data.parentCid",data.parentCid)

        this.$confirm(`是否删除【${data.name}】?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          var catIds = [data.catId];
          console.log("catIds-",catIds);
          this.$http({
            url: this.$http.adornUrl('/product/category/delete/logic'),
            method: 'post',
            data: this.$http.adornData(catIds, false)
          })
          // .then(({data}) => { //删除后的data {msg: 'success', code: 0}  和下面的两种方式都可以，二选一
          //   console.log("删除后的data",data)
          //   this.getMenu();
          //   this.expandedKey=[node.parent.data.catId]
          // })
          .then(() => {
            console.log("删除后的data",data)
            this.getMenu();
            this.expandedKey=[data.parentCid]
          });
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });

      }
    },
    created() {
      this.getMenu();
    }
};
</script>

<style>

</style>
