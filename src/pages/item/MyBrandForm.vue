<template>
  <div>
    <v-form v-model="valid" ref="myBrandForm">
      <v-text-field :rules="nameRules" v-model="brand.name" label="请输入品牌名称"  required></v-text-field>
      <v-text-field :rules="letterRules" v-model="brand.letter" label="请输入品牌首字母" required/>
      <v-cascader
        url="/item/category/list"
        multiple
        required
        v-model="brand.categories"
        label="请选择商品分类"/>

      <v-layout row>
        <v-flex xs3>
          <span style="font-size: 16px; color: #444">品牌LOGO：</span>
        </v-flex>
        <v-flex>
          <v-upload
            v-model="brand.image"
            url="/upload/image"
            :multiple="false"
            :pic-width="250"
            :pic-height="90"
          />
        </v-flex>
      </v-layout>

      <v-layout class="my-4" row>
        <v-spacer/>
        <v-btn @click="submit" color="primary">提交</v-btn>
        <v-btn @click="clear" >重置</v-btn>
      </v-layout>
    </v-form>
  </div>
</template>

<script>
    export default {
        name: "MyBrandForm",
      data(){
        return{
          valid:false,  // 表单校验结果标记
          brand:{
            name:"", // 品牌名称
            letter:"", // 品牌首字母
            image:"", // 品牌logo
            categories:[] // 品牌所属的商品分类数组
          },
          nameRules: [
            v => !!v || "品牌名称不能为空",
            v => v.length > 1 || "品牌名称至少多于2位"
          ],
          letterRules:[
            v=>!!v||"首字母不能为空",
            v => /^[A-Z]{1}$/.test(v) || "品牌字母只能是A~Z的大写字母"
          ]
        }
      },
      methods:{
        submit(){
          console.log("任教")
          // 1、表单校验
          if (this.$refs.myBrandForm.validate()){
            // 2、定义一个请求参数对象，通过解构表达式来获取brand中的属性
            const {categories,letter,...params} = this.brand;
            // 3、数据库中只要保存分类的id即可，因此我们对categories的值进行处理,只保留id，并转为字符串
            params.cids = categories.map(value => value.id).join(",");
            // params.cids = params.cids.split(',');
            console.log("params.cids"+JSON.stringify(params.cids));
            console.log("categories"+JSON.stringify(categories));
            console.log(this.$qs);
            // 4、将字母都处理为大写
            params.letter = letter.toUpperCase();
            //
            this.$http.post('/item/brand',this.$qs.stringify(params))
              .then(value => {
                  this.$message.info("成功");
                  this.$emit("close")

              }).catch(reason => {
                  this.$message.error("错误");
            });
          }
        },
        clear(){
          this.$refs.myBrandForm.reset();

          this.categories=[];

          this.$http.post('/upload/im').then(value => {

          }).catch(reason => {

          })
        }
      }
    }
</script>

<style scoped>

</style>
