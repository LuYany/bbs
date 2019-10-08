<template>
  <div class>
    <div v-if="loading"><text>loading...</text></div>
    <div v-else>
      <div class="top bb" >
        <div style="flex-direction:row;">
          <image style="width:120px;height:120px;" :src="info.avatar_url"></image>
          <text class="loginname">{{info.loginname}}</text>
        </div>
        <text class="jifen">{{info.score}}积分</text>
        <text class="zhuce">注册时间：{{info.create_at}}</text>
      </div>
      <!-- 回复主题 -->
      <div class="bb">
        <text class="zhuti bb">回复的主题</text> 
          <list v-for="(reply,index) in info.recent_replies" :key="index" @click="toArticle(reply)">
            <cell style="flex-direction:row;">
              <text class="aa">{{reply.title.substr(0,22)}}</text>
              <text v-if="reply.title.length > 22">....</text>
            </cell>
          </list> 
      </div>
      <!-- 创建主题 -->
      <div class="bb">
        <text class="zhuti bb">创建的主题</text> 
          <list v-for="(topic,index) in info.recent_topics" :key="index" @click="toArticle(topic)">
            <cell style="flex-direction:row;">
              <text class="aa">{{topic.title.substr(0,22)}}</text>
              <text v-if="topic.title.length > 22">....</text>
            </cell>
          </list> 
      </div>
    </div>
  </div>
</template>

<script>
const stream = weex.requireModule("stream");
const modal = weex.requireModule('modal');
export default {
  name: "userInfo",
  data() {
    return {
      loading: true,
      info:''
    };
  },
  created(){
    this.getInfo();
  },
  methods:{
    getInfo(){
      var that = this;
      stream.fetch({
        method: 'GET',
        url: `https://cnodejs.org/api/v1/user/${this.$route.params.name}`,
        type:'json'
      }, function(ret) {
        if(!ret.ok){
          modal.toast({
            message: 'Network Error!',
            duration: 3
          });
        }else{
          that.loading = false;
          that.info = ret.data.data;
        }
      });
    },
    toArticle(reply){
      this.$router.push({name:"Article",params:{article:reply.id}});
    }
  }
};
</script>

<style scoped>
.loginname{
  font-size: 35px;
  bottom: 0;
  left: 120px;
  position: absolute;
}
.jifen{
  font-size: 35px;
}
.zhuce{
  font-size: 40px;
}
.zhuti{
  background-color:rgba(212, 205, 205, 0.17);
  font-size:33px;
}
.aa{
  color: #094e99;
  margin-bottom: 10px;
  font-size: 30px;
}
.bb{
  padding-top:8px;
  padding-bottom:8px;
  padding-left:8px;
}
</style>
