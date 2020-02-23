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
:root
  font-family 'SF Pro Display', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #242424
  font-weight 400

*,
*::before,
*::after
  margin 0
  box-sizing border-box

.site-content
  width 100vw
  height 100vh
  background white
  @media (min-width 1024px)
    background url(./assets/1920.png)
</style>
