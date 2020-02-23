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
.sign-in
  display flex
  flex-direction column
  align-items flex-start
  width 100%
  &__title
    margin-bottom 14px
    font-size 16px
    line-height 20px
    color #404040
  &__input
    margin-bottom 10px
    padding 0 19px
    width 100%
    height 65px
    font-size 18px
    line-height 20px
    border 1px solid #DCE4EE
    @media (min-width 1024px)
      padding 0 27px
    &--error
      border-color #DB5454
  &__confirm
    margin-bottom 50px
    width 100%
    height 65px
    font-size 16px
    line-height 20px
    color white
    border none
    outline none
    text-decoration none
    transition all 0.3s ease-out 0s
    background #2F80F3
    cursor pointer
    &:hover
      opacity 0.8
    &--disabled
      background #DCE4EE
      cursor not-allowed
  &__warning
    position absolute
    width 290px
    top 290px
    font-size 16px
    line-height 16px
    transition all 0.1s ease-out 0s
    color #DB5454
    @media (min-width 1024px)
      width 412px
    &--hidden
      opacity 0
  &__accept
    font-size 14px
    line-height 20px
  &__accept-link
    color inherit
    text-decoration underline
</style>
