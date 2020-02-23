<template>
  <div id="app">
    <div class="site-content" />
    <lightbox :title="title">
      <component
        :is="activeView"
        :phone.sync="phone"
        :code="code"
        @setPhone="verifyPhone"
        @changePhone="changePhone"
        @resendCode="generateCode"
      />
    </lightbox>
  </div>
</template>

<script>
import Lightbox from '@/components/modal/Lightbox.vue';
import SmsAuth from '@/components/modal/SmsAuth.vue';
import SignIn from '@/components/modal/SignIn.vue';

export default {
  name: 'App',
  components: {
    Lightbox,
    SmsAuth,
    SignIn,
  },
  data() {
    return {
      activeView: 'SignIn',
      phone: '',
      code: '',
    };
  },
  computed: {
    title() {
      return this.activeView === 'SignIn'
        ? 'Вход или регистрация'
        : 'Введите код';
    },
  },
  methods: {
    generateCode() {
      this.code = String(Math.floor(Math.random() * 10000)).padStart(4, '0');
    },
    verifyPhone() {
      setTimeout(() => {
        this.generateCode();
        this.activeView = 'SmsAuth';
      }, 1200);
    },
    changePhone() {
      this.activeView = 'SignIn';
    },
  },
};
</script>

<style lang="stylus">
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
</style>
