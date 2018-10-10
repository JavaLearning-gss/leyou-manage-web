<template>
  <div>
    <v-layout row wrap class="px-3" ><!--使用栅格系统的时候，每一行会被划分成12个栅格-->
      <v-flex xs2>
        <!--让一个单元格占4个栅格-->
        <v-btn color="info">新增品牌</v-btn>
      </v-flex>
      <v-spacer/><!--用该标签占据其他栅格-->
      <v-flex xs4>
        <v-text-field
          placeholder="Search"
          append-icon="search"
          hide-details
          v-model="key"
        ></v-text-field>
      </v-flex>
    </v-layout>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center">
          <img :src="props.item.image" v-if="props.item.image"/>
          <span v-else>无图片</span>
        </td>
        <td class="text-xs-center">{{ props.item.letter }}</td>

        <td class="text-xs-center">
          <v-btn  flat icon color="grey">
            <v-icon>edit</v-icon>
          </v-btn>
            <v-btn flat icon color="grey">
              <v-icon>delete</v-icon>
            </v-btn>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
    export default {
        name: "my-brand.vue",
      data(){
          return{
            totalBrands: 0,
            loading: true,//当该属性为true时显示加载，不为ture是加载完成
            pagination:{},
            headers:[//headers属性定义的表头每一列的信息，也就是表的字段信息
              {text: '品牌id',//该属性为用户看到的
              align: 'center',
              sortable: false,
              value: 'id'//该属性定义的是列的字段，与数据库的字段一致
            },
              { text: '名称', value: 'name',sortable: false,align: 'center' },
              { text: 'logo', value: 'image',sortable: false,align: 'center' },
              { text: '首字母', value: 'letter',sortable: true,align: 'center'},
              { text: '操作',align: 'center' }],
            brands:[],
            key:""
          }
      },
      created(){

          //当组件创建完毕的时候就要去加载数据
        //调用加载数据的方法，所以要创建该方法
        this.loadData();
      },
      methods:{
          loadData(){
            this.loading=true;
            //发送异步请求，使用axios发送,返回的是一个promise对象
            $http.get(
              "/item/brand/page",
              {params:{
                page:this.pagination.page,
                  rows:this.pagination.rowsPerPage,
                  sortBy:this.pagination.sortBy,
                  desc:this.pagination.descending,
                  key:this.key
                }}
            ).then(
              resp=>{
                this.brands=resp.data.items;
                this.totalBrands=resp.data.total;
              }
            ).catch(error=>{
                console.log(error);
              }

            );
            this.loading=false;
          },
        watch:{
            //需要深度监视pagination对象，如果pagination对象的属性发生改变就重新加载
            pagination:{
              deep:true,
              handle(){
                this.loadData();
              }
            },
          //还需要监视key属性
          key(){
            //当key属性发生变化时，页重新加载数据
            this.loadData();
          }
        }

      }
    }
</script>

<style scoped>

</style>
