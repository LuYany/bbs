<template>
  <div>
      <div class="search">
        <input type="text" v-model="keywords" placeholder="输入关键字搜索" @keyup.enter="serchLists">
        <button @click="serchLists()">搜索</button>
      </div>
      <div v-if="!lists.length"><text>loading...</text></div>
      <list v-if="lists.length">
        <cell v-for="(list,index) in lists2" :key="index">
          <div class="list">
            <image :src="list.author.avatar_url" style="width:40px;height:40px;" @click="toUserInfo(list)"></image>
            <text class="flex1-5 center font28 both">{{list.reply_count}}/{{list.visit_count}}</text>
            <div class="flex4" style="flex-direction:row;" @click="toArticle(list)">
              <text  class="font38">{{list.title.substr(0,14)}}</text>
              <text  v-if="list.title.length > 14">......</text>
            </div>
            
            <text  class="flex1 center font28 both">{{list.create_at | formatDate}}</text>
          </div>
        </cell>
      </list>
  </div>
</template>

<script>
const stream = weex.requireModule("stream");
const modal = weex.requireModule('modal');

export default {
  name: "HelloWorld",
  data() {
    return {
      lists:[],
      lists2:[],
      loading: true,
      keywords:''
    };
  },
  created(){
    this.getLists();
  },
  computed:{
    filterLists(){
        return this.lists.filter((list)=>{
          return list.title.match(this.keywords)
        })
    }
  },
  methods: {
    // 获取列表
    getLists() {
      var that = this;
      stream.fetch({
        method: 'GET',
        url: "https://cnodejs.org/api/v1/topics",
        type:'json'
      }, function(ret) {
        if(!ret.ok){
          modal.toast({
            message: 'Network Error!',
            duration: 3
          });
        }else{
          that.lists = ret.data.data;
          that.loading = false;
          that.lists2 = that.lists;
        }
      });
    },
    // 进个人信息
    toUserInfo(list){
      this.$router.push({name:'UserInfo',params:{name:list.author.loginname}});
    },
    // 进详情
    toArticle(list){
      this.$router.push({name:'Article',params:{article:list.id}});
    },
    serchLists() {
      this.lists2 = this.lists;
      console.log(this.lists2)
      this.lists2 =  this.filterLists;
      console.log(this.lists2)
    }
  }
};
</script>

<style scoped>
  .bell {
    font-family: iconfont;
    font-size: 36px;
  }
  .search{
    background-color: blanchedalmond;
    padding: 10px;
  }
  .list{
    flex-direction: row;
    justify-content: space-around;
    border-bottom-width: 1px;
    border-bottom-style: solid;
    border-bottom-color: #e7e7e7;
    padding-top: 15px;
    padding-bottom: 15px;
    margin-left: 20px;
    margin-right: 20px;
  }
  .flex-7{
    flex:.7;
  }
  .flex1{
    flex: 1;
  }
  .flex1-5{
    flex: 1.5;
  }
  .flex4{
    flex: 4;
  }
  .center{
    text-align: center;
  }
  .font28{
    font-size: 24px;
    color: #858585;
  }
  .font38{
    font-size: 30px;
  }
  .both{
    line-height: 44px;
  }
</style>
