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
</style>
