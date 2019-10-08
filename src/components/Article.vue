<template>
  <div>
    <div v-if="loading">
      <text>loading...</text>
    </div>
    <div v-else>
        <text class="title">{{info.title}}</text>
        <div class="desp">
          <text class="color">·分类：{{tab}}</text>
          <text class="color">·{{info.visit_count}}次访问</text>
          <text class="color">·发布于：{{info.create_at | formatDate}}</text>
          <text class="color" @click="toUserInfo(info)">·作者：{{info.author.loginname}}</text>
        </div>
        <!-- 内容 -->
        <!-- <text>{{info.content}}</text> -->
        <div class="markdown-text">
          <text>更灵活的机动、更快的效率、更高的产能↵Serverless - 解放的不仅仅是生产力，更是一种全新的思维方式。</text>
          <text>百姓网 Hax、上线了郭达峰、ThoughtWorks 陈欢、阿里云杜万、淘宝冰森等优秀分享者，↵期待与你一起进入嗨翻 server 的新场景。</text>
          <!-- <image src="//static.cnodejs.org/FlbUfz7IHVgS4DwmjSrXKiL9fMRl" style="width:600px;height:450px;" alt="image.png"></image> -->
          <!-- <image src="//static.cnodejs.org/FlDAitTvYWpWsudI8VGjfjGGXYRr" style="width:600px;height:450px;" alt="image.png"></image> -->
          <text>时间：03 月 16 日周六</text>
          <text>地点：上海证大五道口广场</text>
          <text>报名链接：</text>
          <a href="https://survey.alibaba.com/apps/zhiliao/tZc2WjLe2"><text>https://survey.alibaba.com/apps/zhiliao/tZc2WjLe2</text></a>
          <text>活动主页：</text>
          <a href="https://taobaofed.github.io/nodejs-club/"><text>https://taobaofed.github.io/nodejs-club/</text></a>
        </div>
        <!-- 评论 -->
        <scroller>
          <list>
            <cell v-for="(reply,index) in info.replies" :key="index">
              <div class="aa">
                <image :src="reply.author.avatar_url" style="width: 50px;height:50px;" @click="toUserInfo(reply)"></image>
                <div class="bb">
                  <text class="loginname">{{reply.author.loginname}}</text>
                  <text class="lou">{{index+1}}楼</text>
                </div>
                <text class="red xin">❤{{reply.ups.length}}</text>
              </div>
              <!-- <text>{{reply.content}}</text> -->
              <div class="markdown-text">
                <text class="content">参加。聆听大牛们的代码。</text>
              </div>
            </cell>
          </list>  
        </scroller>
    </div>
  </div>

</template>

<script>
const stream = weex.requireModule("stream");
const modal = weex.requireModule("modal");

export default {
  name: "",
  data() {
    return {
      loading: true,
      info: ""
    };
  },
  created() {
    this.getInfo();
  },
  computed:{
    // 转换分类值
    tab(){
      var tab = this.info.tab;
      if(tab == "share"){
        return "分享";
      }else if(tab == "ask"){
        return "问答";
      }else if(tab == "job"){
        return "招聘";
      }else{
        return "精品";
      }
    }
  },
  methods: {
    getInfo() {
      var that = this;
      stream.fetch(
        {
          method: "GET",
          url: `https://cnodejs.org/api/v1/topic/${this.$route.params.article}`,
          type: "json"
        },
        function(ret) {
          if (!ret.ok) {
            modal.toast({
              message: "Network Error!",
              duration: 3
            });
          } else {
            console.log(ret.data.data);
            that.info = ret.data.data;
            that.loading = false;
          }
        }
      );
    },
    toUserInfo(xinxi){
      this.$router.push({name:'UserInfo',params:{name:xinxi.author.loginname}});
    }
  }
  // filters:{
  //   formatDate(str){
	// 	    if (!str) return ''
	// 	    var date = new Date(str)
	// 	    var time = new Date().getTime() - date.getTime() //现在的时间-传入的时间 = 相差的时间（单位 = 毫秒）
	// 	    if (time < 0) {
	// 	        return ''
	// 	    } else if ((time / 1000 < 30)) {
	// 	        return '刚刚'
	// 	    } else if (time / 1000 < 60) {
	// 	        return parseInt((time / 1000)) + '秒前'
	// 	    } else if ((time / 60000) < 60) {
	// 	        return parseInt((time / 60000)) + '分钟前'
	// 	    } else if ((time / 3600000) < 24) {
	// 	        return parseInt(time / 3600000) + '小时前'
	// 	    } else if ((time / 86400000) < 31) {
	// 	        return parseInt(time / 86400000) + '天前'
	// 	    } else if ((time / 2592000000) < 12) {
	// 	        return parseInt(time / 2592000000) + '月前'
	// 	    } else {
	// 	        return parseInt(time / 31536000000) + '年前'
	// 	    }
  //   }
  // }
};
</script>

<style scoped>
.title {
  font-size: 32px;
  margin-bottom: 20px;
  font-weight: bold;
}
.color {
  color: #a8a3a3;
  font-size: 25px;
}
.markdown-text{
  margin-top: 30px;
  margin-bottom: 80px;
}
.aa{
  flex-direction: row;
}
.red{
  color: red;
  float: right;
}
.xin{
  font-size: 28px;
  bottom: 0;
  right: 0;
  position: absolute;
}
.bb{
  flex-direction: row;
  bottom: 0;
  left: 80px;
  position: absolute;
}
.loginname{
  font-size: 28px;
  font-weight: bold;
  margin-left: 10px;
}
.lou{
  font-size: 24px;
  color: #806767;
  margin-left: 10px;
}
.content{
  font-size: 32px;
  margin-left: 30px;
}
</style>
