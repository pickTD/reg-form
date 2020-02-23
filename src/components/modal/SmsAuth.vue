<template>
  <div class="sms-auth">
    <p class="sms-auth__title">
      {{ `Мы отправили код на ${phone}` }}
    </p>
    <p
      class="sms-auth__back"
      @click="$emit('changePhone')"
    >
      <span>Изменить</span>
    </p>
    <div class="sms-auth__code">
      <input
        v-for="(i, index) in code"
        :key="index"
        v-model="codeLocal[index]"
        ref="code"
        class="sms-auth__code-item"
        type="text"
        :placeholder="i"
        @keyup.left="focusLeft(index)"
        @keyup.right="focusRight(index)"
        @keypress="validateValue($event, index)"
        @input="handleInput($event, index)"
        @paste="handlePaste($event)"
      >
    </div>
    <p
      class="sms-auth__warning"
      :class="{ 'sms-auth__warning--hidden': isCodeValid }"
    >
      Вы ввели неправильный код, попробуйте еще раз
    </p>
    <div class="sms-auth__timer">
      <p v-if="timer && timeLeft">
        Получить новый код можно<br>через 0:{{ String(timeLeft).padStart(2, '0') }}
      </p>
      <p
        v-else
        class="sms-auth__timer--resend"
        @click="$emit('resendCode')"
      >
        Отправить код еще раз
      </p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'SmsAuth',
  props: {
    phone: { type: String, default: '' },
    code: { type: String, default: '' },
  },
  data() {
    return {
      codeLocal: Array(4).fill(''),
      isCodeValid: true,
      timer: null,
      timeLeft: 59,
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.focus(0);
      this.startTimer();
    });
  },
  watch: {
    code() {
      this.startTimer();
    },
    codeLocal(newVal) {
      const strVal = newVal && newVal.join('');
      this.isCodeValid = strVal.length < 4 || strVal === this.code;
      if (strVal === this.code) {
        this.stopTimer();
        this.timer = null;
        this.$emit('phoneConfirmed');
      }
    },
  },
  methods: {
    focus(index) {
      this.$refs.code[index].focus();
    },
    focusLeft(index) {
      if (index > 0) {
        this.focus(index - 1);
      }
    },
    focusRight(index) {
      if (index < 3) {
        this.focus(index + 1);
      }
    },
    validateValue(event, index) {
      if (this.codeLocal[index] || !/\d/.test(event.key)) {
        event.preventDefault();
      }
    },
    handleInput({ data }, index) {
      if (data) {
        const emptyIdx = this.codeLocal.slice(index).findIndex((item) => !item);
        if (~emptyIdx) {
          this.focus(emptyIdx + index);
        }
      } else if (!data && index > 0) {
        this.focusLeft(index);
      }
    },
    handlePaste(event) {
      const eventData = event.clipboardData.getData('text/plain');
      let data = eventData && eventData
        .split('')
        .filter((char) => /\d/.test(char))
        .slice(0, 4);
      if (data && data.length) {
        if (data.length < 4) {
          data = [...data, ...Array(4 - data.length).fill('')];
        }
        this.codeLocal = data;
      }
      event.preventDefault();
    },
    startTimer() {
      this.timeLeft = 59;
      this.timer = setInterval(() => {
        if (this.timeLeft === 1) this.stopTimer();
        this.timeLeft -= 1;
      }, 1000);
    },
    stopTimer() {
      clearInterval(this.timer);
    },
  },
};
</script>

<style scoped lang="stylus">
.sms-auth
  display flex
  flex-direction column
  align-items flex-start
  width 100%
  &__title
    margin-bottom 9px
    font-size 14px
    line-height 20px
    @media (min-width 1024px)
      margin 16px 0 10px
      font-size 20px
  &__back
    margin-bottom 29px
    font-size 14px
    line-height 20px
    color #2F80F3
    cursor pointer
    @media (min-width 1024px)
      margin-bottom 35px
      font-size 20px
  &__code
    margin-bottom 66px
    display flex
    justify-content space-between
    width 100%
    @media (min-width 1024px)
      margin-bottom 49px
    &-item
      width 65px
      height 65px
      font-size 30px
      line-height 20px
      text-align center
      border 1px solid #DCE4EE
      @media (min-width 1024px)
        width 90px
        height 80px
        font-size 40px
        border 1px solid rgba(47, 128, 243, 0.32)
      &::placeholder
        color #F8F9F9
  &__warning
    position absolute
    width 290px
    top 259px
    font-size 16px
    line-height 17px
    transition all 0.1s ease-out 0s
    color #DB5454
    @media (min-width 1024px)
      width 412px
      top 299px
      line-height 20px
    &--hidden
      opacity 0
  &__timer
    align-self center
    font-size 16px
    line-height 20px
    text-align center
    color #788AA4
    &--resend
      cursor pointer
</style>
