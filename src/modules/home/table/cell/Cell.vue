<template>
  <li class="flex Cell">
    <span class="Content">
      <strong>{{ item.title }}: </strong>{{ item.description }}
    </span>
    <span class="Score">
      <strong>{{ item.score || 1 }}</strong>
    </span>
  </li>
</template>

<script>
export default {
  name: 'Cell',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped lang="scss">
@import 'style/_variables.scss';

.Cell {
  padding: 1rem;
}

.Content {
  width: 100%;
}

.Score {
  padding: 0 1rem;
  color: $brand-primary;
}
</style>
