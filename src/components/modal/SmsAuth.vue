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
      timeLeft: 60,
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
    startTimer() {
      this.timeLeft = 60;
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
</style>
