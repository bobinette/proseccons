<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app',
};
</script>

<style lang="scss">
@import 'style/base.scss';

#app {
  margin-bottom: 1rem;
}
</style>
