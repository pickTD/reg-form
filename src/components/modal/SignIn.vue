<template>
  <div class="sign-in">
    <p class="sign-in__title">
      Номер телефона
    </p>
    <input
      class="sign-in__input"
      :class="{ 'sign-in__input--error': !isPhoneValid }"
      v-mask="'+7 (###) ###-##-##'"
      placeholder="+7 (___) ___-__-__"
      type="tel"
      ref="input"
      v-model="phoneLocal"
      @keyup.enter="setPhone"
    >
    <button
      class="sign-in__confirm"
      :class="{ 'sign-in__confirm--disabled': !isPhoneValid || isCodeLoading }"
      :disabled="!isPhoneValid"
      @click="setPhone"
    >
      <span v-if="!isCodeLoading">Продолжить</span>
      <span v-else>Загрузка ...</span>
    </button>
    <p
      class="sign-in__warning"
      :class="{ 'sign-in__warning--hidden': isPhoneValid }"
    >
      Для продолжения введите корректный номер телефона
    </p>
    <p class="sign-in__accept">
      Нажимая кнопку «Продолжить», я даю согласие на обработку
       моих персональных данных и принимаю настоящие
      <a
        class="sign-in__accept-link"
        href="#"
      >
        Соглашения
      </a>
    </p>
  </div>
</template>

<script>
export default {
  name: 'SignIn',
  props: {
    phone: { type: String, default: '' },
  },
  data() {
    return {
      phoneLocal: '',
      isPhoneValid: false,
      isCodeLoading: false,
    };
  },
  mounted() {
    if (this.phone) {
      this.phoneLocal = this.phone;
    }
    this.$nextTick(() => {
      this.$refs.input.focus();
    });
  },
  watch: {
    phoneLocal(newVal) {
      this.isPhoneValid = newVal && newVal.length === 18;
    },
  },
  methods: {
    setPhone() {
      if (this.isPhoneValid) {
        this.isCodeLoading = true;
        this.$emit('update:phone', this.phoneLocal);
        this.$emit('setPhone');
      }
    },
  },
};
</script>

<style scoped lang="stylus">
</style>
