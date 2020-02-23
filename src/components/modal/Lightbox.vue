<template>
  <div class="lightbox">
    <div class="lightbox__content">
      <div class="lightbox__wrap">
        <div class="lightbox__close" />
        <h4 class="lightbox__title">{{ title }}</h4>
        <slot />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Lightbox',
  props: {
    title: { type: String, default: '' },
  },
};
</script>

<style scoped lang="stylus">
.lightbox
  position fixed
  z-index 3
  display flex
  flex-flow row nowrap
  justify-content center
  align-items center
  overflow-y scroll
  width 100vw
  height 100vh
  top 0
  left 0
  background-color rgba(249, 250, 253, 0.91)

  &__content
    position relative
    padding 52px 15px 0 15px
    width 100%
    height 100%
    background white
    @media (min-width 1024px)
      padding 54px 60px 0 48px
      width 520px
      height 419px
      box-shadow 0px 7px 70px rgba(0, 50, 123, 0.1)

  &__wrap
    width 290px
    margin 0 auto
    @media (min-width 1024px)
      width 100%

  &__close
    position absolute
    width 17px
    height 17px
    top 15px
    right 17px
    background url('../../assets/close.svg')
    cursor pointer
    transition all 0.3s ease-out 0s
    @media (min-width 1024px)
      top 26px
      right 24px
    &:hover
      transform rotate(90deg)

  &__title
    margin-bottom 38px
    text-align start
    font-style normal
    font-weight 500
    font-size 22px
    line-height 20px
    @media (min-width 1024px)
      font-size 25px
</style>
