<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
import Authing from 'authing-js-sdk';
// console.log('Authing :>> ', Authing);

export default {
  name: 'App',
  mounted() {
    localStorage.setItem('token','aaabbcc112233')
    this.checkLogin();
  },
  methods: {
    async checkLogin() {
      const token = localStorage.getItem('token');
      console.log('token :>> ', token);

      if (token) {
        const userPoolId = '';

        const authing = new Authing({
          userPoolId,
        });

        const result = await authing.checkLoginStatus(JSON.parse(token));
        console.log('result :>> ', result);

        if (result.status) {
          const userInfo = localStorage.getItem('userInfo');

          if (userInfo) {
            this.$store.commit('SET_USER', JSON.parse(userInfo));
          }
        } else {
          localStorage.removeItem('token');
          localStorage.removeItem('userInfo');
        }
      }
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
