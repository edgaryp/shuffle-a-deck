<template>
    <div class="card" :class="suitColor[card.suit]" :key="index">
      <span class="card__suit card__suit--top">{{ card.suit }}</span>
      <span class="card__number">{{ card.rank }} </span>
      <span class="card__suit card__suit--bottom">{{ card.suit }}</span>
    </div>
</template>

<script>
export default {
  props: [
    'card',
    'suitColor',
    'index'
  ]
};
</script>

<style lang="less" scoped>
</style>
