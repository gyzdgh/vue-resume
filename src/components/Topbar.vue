<template>
  <div id="topbar">
    <div class="wrapper">
      <span class="logo">Resumer</span>
      <div class="actions">
        <div v-if="logined" class="userActions">
          <span class="welcome">你好，{{user.username}}</span>
          <a class="button" href="#" @click.prevent="signOut">登出</a>
        </div>
        <div v-else class="userActions">
          <a class="button" href="#" @click.prevent="signUpDialogVisible = true">注册</a>
          <a class="show" href="#" @click.prevent="signInDialogVisible = true">登录</a>
        </div>
      </div>
    </div>
    <!-- 注册/登陆对话框 -->
    <MyDialog title="注册" :visible="signUpDialogVisible" @close="signUpDialogVisible = false">
      <SignUpForm @success="signIn($event)"/>
    </MyDialog>
    <MyDialog title="登录" :visible="signInDialogVisible"
      @close="signInDialogVisible = false">
      <SignInForm @success="signIn($event)"/>
    </MyDialog>
  </div>
</template>

<script>
//引入 对话框
import MyDialog from './MyDialog'
//引入 注册对话框
import SignUpForm from './SignUpForm'
//引入登陆对话框
import SignInForm from './SignInForm'
//引入数据库
import AV from '../lib/leancloud'

export default {
  name: 'Topbar',
  data(){
    return {
      //初始化 注册/登陆框的默认状态为 false
      signUpDialogVisible: false,
      signInDialogVisible: false,
    }
  },
  computed: {
    user(){
      return this.$store.state.user
  },
    logined(){
      return this.user.id
    }
  },
  components: {
     MyDialog, SignUpForm, SignInForm
  },
  methods: {
    //登出
    signOut(){
      AV.User.logOut()
      this.$store.commit('removeUser')
    },
    //登入
    signIn(user){
      this.signUpDialogVisible = false
      this.signInDialogVisible = false
      this.$store.commit('setUser', user)
    },
  }
};
</script>

<style scoped lang="scss">    //scoped 表示只作用当前组件的元素
#topbar {
  background: #ffffff;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.25);
  > .wrapper {
    min-width: 1024px;
    max-width: 1440px;
    margin: 0 auto;
    height: 64px;
  }
  > .wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 16px;
  }
  .logo {
    font-size: 24px;
    color: #000000;
  }
}
.button,
.sign,
.show {  // 由于加了 scoped， 所以这个 button 选择器只在本组件内有效，不会影响其他组件
  width: 72px;
  height: 32px;
  border: none;
  cursor: pointer;
  font-size: 18px;
  background: #ddd;
  color: #222;
  border-radius: 5px;
  outline-style: none;
  text-decoration: none;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  vertical-align: middle;
  &.sign{
    margin-right: 30px;
  }
  &.show{
    background:#4B8AEC
  }
  &:hover {
    box-shadow: 1px 1px 1px hsla(0, 0, 0, 0.5);
  }
}
.actions{
    display: flex;
    .userActions{
      margin-right: 3em;
      .welcome{
        margin-right: .5em;
      }
    }
}
</style>