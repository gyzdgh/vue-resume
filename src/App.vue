<template>
  <!-- 在页面添加三大块使用flex -->
  <div>
    <div class=page>
      <header>
        <Topbar/>
      </header>
      <main>
        <ResumeEditor/>
        <ResumePreview/>
      </main>
    </div>
  </div>
</template>

<script>
// 引入 normalize.css
import "normalize.css/normalize.css";
// 引入 reset.css 重置样式
import "./assets/reset.css";

//引入三大块
import Topbar from "./components/Topbar";
import ResumeEditor from "./components/ResumeEditor";
import ResumePreview from "./components/ResumePreview";
//引入图标
import icons from "./assets/icons";

//引入原有数据
import store from './store/index'
//引入数据库
import AV from './lib/leancloud'
//获取用户
import getAVUser from './lib/getAVUser'

export default {
  name: "app",
  store,
  //三大块
  components: { Topbar, ResumeEditor, ResumePreview},
  //创建icon图标
  created() {
    document.body.insertAdjacentHTML("afterbegin", icons);
    //数据保存到 localStorage
    let state = localStorage.getItem('state')
      if(state){
        state = JSON.parse(state) 
      }
      this.$store.commit('initState', state)
      this.$store.commit('setUser', getAVUser())
  }
};
</script>

<style lang="scss">
  .page{
    height: 100vh;
    display: flex;
    flex-direction: column;
    background: #EAEBEC;
    >main{
      flex-grow: 1;  
    }
    >main{
      min-width: 1024px;
      max-width: 1440px;
      margin-top: 16px;
      margin-bottom: 16px;
      display: flex;
      justify-content: space-between;
      padding: 0 16px;
      width: 100%; 
      align-self: center;
    }
  }
  #resumeEditor{
    min-width: 35%;
    background: #444;
  }
  #resumePreview{
    flex-grow: 1;
    margin-left: 16px;
    background: #777;
  }
  svg.icon{
    height: 1em;
    width: 1em;
    fill: currentColor;
    vertical-align: -0.1em;
    font-size:16px;
  }
</style>