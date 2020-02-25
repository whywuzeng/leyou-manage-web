<template>
  <v-card>
      <v-flex xs12 sm10>
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>
  export default {
    name: "category",
    data() {
      return {
        isEdit:true,
        treeData:[
          {
            "id": 74,
            "name": "手机",
            "parentId": 0,
            "isParent": true,
            "sort": 2
          },
          {
            "id": 75,
            "name": "家用电器",
            "parentId": 0,
            "isParent": true,
            "sort": 3
          }
        ]
      }
    },
    methods: {
      handleAdd(node) {
        console.log("add .... ");
        console.log(node);
        if (node.parentId!==0)
        {
          //验证verify
          // this.verify().then(() => {
          //
          // }).catch(() => {
          //   this.$router.push("/login");
          // });

          this.$http.post('/item/category',this.$qs.stringify(node))
            .then(value => {
              this.reloadData(node.id);
            }).catch();
        }else {
          this.$message.error("刷新后重试!")
        }
      },
      handleEdit(id, name) {
        console.log("edit... id: " + id + ", name: " + name)
      },
      handleDelete(id) {
        console.log("delete ... " + id)
      },
      handleClick(node) {
        console.log(node)
      },
      reloadData(id) {
        //操作完成后刷新数据
        this.$http.get("/item/category/list?pid="+id).then().catch();
      }
    }
  };
</script>

<style scoped>

</style>
