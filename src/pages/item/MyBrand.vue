<template>
  <div>
    <v-card>
      <v-card-title>
    <v-btn depressed color="primary" @click="addBrand">新增</v-btn>
        <!--搜索框，与search属性关联-->
        <!--空间隔离组件-->
        <v-spacer />
        <v-text-field label="输入关键字搜索" v-model="search" append-icon="search" hide-details/>
        </v-card-title>
      <!-- 分割线 -->
      <v-divider/>
    <v-data-table
      :headers="headers"
      :items="brands"
      :search="search"
      :pagination.sync="pagination"
      :server-items-length="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td>{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image"></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="justify-center layout">
          <v-btn depressed color="primary">修改</v-btn>
          <v-btn depressed color="error">删除</v-btn>
        </td>
      </template>
    </v-data-table>
    </v-card>

    <!--弹出的对话框-->
    <v-dialog max-width="500" v-model="show" persistent>
      <v-card>
        <!--对话框的标题-->
        <v-toolbar dense dark color="primary">
          <v-toolbar-title>新增品牌</v-toolbar-title>
          <!--空间隔离组件-->
          <v-spacer />
          <!--关闭窗口的按钮-->
          <v-btn icon @click="closeWindow"><v-icon>close</v-icon></v-btn>
        </v-toolbar>
        <!--对话框的内容，表单-->
        <v-card-text class="px-5">
          <my-brand-form @close="closeWindow"></my-brand-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  import MyBrandForm from './MyBrandForm'
    export default {
        name: "MyBrand",
      data(){
          return {
            search: '', // 搜索过滤字段
            totalBrands: 0, // 总条数
            brands: [], // 当前页品牌数据
            loading: true, // 是否在加载中
            pagination: {}, // 分页信息
            headers: [ // 头信息
              {text: 'id', align: 'center', value: 'id'},
              {text: '名称', align: 'center', sortable: false, value: 'name'},
              {text: 'LOGO', align: 'center', sortable: false, value: 'image'},
              {text: '首字母', align: 'center', value: 'letter', sortable: true,},
              {text: '操作', align: 'center', value: 'id', sortable: false}
            ],
            show:false,
          }
      },
      components:{
        MyBrandForm
      },
      mounted(){ // 渲染后执行
        // 查询数据
        this.getDataFromServer();
      },
      methods:{
        getDataFromServer() { // 从服务的加载数据的方法。
          // {"descending":false,"page":1,"rowsPerPage":5,"sortBy":"id","totalItems":5}
          // 伪造假数据
          this.$http.get("/item/brand/page", {
           params: {
             page: this.pagination.page,
             rows: this.pagination.rowsPerPage,
             sortBy: this.pagination.sortBy,
             isDesc: this.pagination.descending,
             key: this.search
           }
          })
            .then(resp => {
              this.totalBrands = resp.data.totalPage;
              this.brands = resp.data.items;
              this.loading = false;
            })
        },
        addBrand(){
          this.show = true;
        },
        closeWindow(){
          this.show = false;
        }
      }
    }
</script>

<style scoped>

</style>
