<template>
  <div id="q-app">
    <div class="main">
    <transition name="fade">
      <router-view v-on:login="login" v-on:logout="logOut" v-on:register="register" :logged="loggedIn" :user="user"/>
    </transition>
  </div>
</div>
</template>

<script>
export default {
  name: 'App',
  created () {
    let vue = this
    vue.user.token = localStorage.getItem('token')
    vue.user.id = localStorage.getItem('userId')
    vue.user.name = localStorage.getItem('name')
    vue.user.admin = localStorage.getItem('admin')
    if (vue.user.token !== null) {
      vue.loggedIn = true
    }
  },
  data: function () {
    return {
      loggedIn: false,
      user: {
        id: '',
        token: '',
        name: '',
        admin: false
      }
    }
  },
  methods: {
    login: function (user) {
      let vue = this
      vue.user.token = user.token
      vue.user.id = user.id
      vue.user.admin = user.admin
      vue.loggedIn = true
      vue.$router.push('/')
    },
    logOut: function () {
      let vue = this
      vue.user.token = ''
      vue.user.id = ''
      vue.loggedIn = false
      localStorage.removeItem('token')
      localStorage.removeItem('userId')
      localStorage.removeItem('name')
      localStorage.removeItem('admin')
      vue.$router.push('/')
    },
    register: function (user) {
      let vue = this
      vue.user.token = user.token
      vue.user.id = user.id
      vue.user.name = user.name
      vue.user.admin = user.admin
      vue.$router.push('/account')
    }
  }
}
</script>

<style scoped lang="less">
  .fade-enter-active, .fade-leave-active {
    transition: all .25s ease;
    transition: all .25s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }

  .fade-enter, .fade-leave-active {
    opacity: 0;
    transform: translateY(20px);
  }
</style>
