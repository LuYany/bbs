<template>
  <div class="wrapper">
    <Header></Header>
    <router-view/>
  </div>
</template>

<script>
const storage = weex.requireModule('storage');
const modal = weex.requireModule('modal');
import Header from './components/Header'
export default {
  name: 'App',
  data () {
    return {
      
    }
  },
  components:{Header},
  created(){
    var domModule = weex.requireModule("dom");
    domModule.addRule("fontFace", {
      fontFamily: "iconfont", 
      src: "url('//at.alicdn.com/t/font_1098968_xnuygsfj4g.ttf')"
      // src: "url('//at.alicdn.com/t/font_1098968_jaoiujmr0t.woff')"
    });
  }
}
</script>

<style scoped>

</style>
