<template>
  <div class="horizontal-list">
    <article v-for="item in items" :key="item.id">
      <UiHorizontalListItem :item="item" />
    </article>
  </div>
</template>

<script>
export default {
  name: 'HorizontalList',
  props: {
    items: {
      type: Array,
      default: () => [],
      required: true,
    },
  },
}
</script>

<style lang="scss" scoped>
.horizontal-list {
  color: rgba(#000, 0.87);
  line-height: 1.5;
  display: flex;
  overflow-y: auto;
}
article {
  flex: 0 0 auto;
  width: 100%;
  max-width: 190px;
  @include media-breakpoint-up(md) {
    max-width: 342px;
  }
  + article {
    margin-left: 15px;
    @include media-breakpoint-up(md) {
      margin-left: 35px;
    }
  }
  &:last-child {
    padding-right: 20px;
    box-sizing: content-box;
    @media (min-width: 1096px) {
      padding-right: calc((100vw - 1096px) / 2 + 20px);
    }
    @media (min-width: 1136px) {
      padding-right: calc((100vw - 1096px) / 2);
    }
  }
}
</style>
