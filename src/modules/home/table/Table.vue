<template>
  <div class="Proseccon">
    <h2>{{ title }}</h2>
    <div class="container card Table">
      <!-- Pros and cons columns -->
      <div class="row">
        <div class="col-md-6 Column">
          <ul>
            <cell v-for="(item, idx) in pros" :item="item" :key="idx"></cell>
          </ul>
        </div>
        <div class="col-md-6 Column">
          <ul>
            <cell v-for="(item, idx) in cons" :item="item" :key="idx"></cell>
          </ul>
        </div>
      </div>

      <!-- New proseccon input -->
      <input type="text" v-model="proseccon" @keyup.13="addProseccon" placeholder="Add a new item in the format &quot;title: description ±1&quot;">
    </div>
  </div>
</template>

<script>
import Cell from './cell/Cell';

export default {
  name: 'Table',
  props: {
    title: String,
    pros: {
      type: Array,
      default: [],
    },
    cons: {
      type: Array,
      default: [],
    },
  },
  data() {
    return {
      proseccon: '',
    };
  },
  methods: {
    addProseccon() {
      this.$emit('new-proseccon', this.proseccon);
      this.proseccon = '';
    },
  },
  components: {
    Cell,
  },
};
</script>

<style scoped lang="scss">
@import 'style/_variables.scss';

.Table {
  padding: 1rem;

  input {
    background: transparent;
    outline: none;
    border: none;
    border-bottom: 1px solid $gray-lighter;
    width: 100%;

    &:hover {
      border-bottom-color: lighten($brand-primary, 30);
    }

    &:focus {
      border-bottom-color: $brand-primary;
    }
  }
}

.Column {
  & h3 {
    text-align: center;
    margin: 0;
    padding: 1rem 0;
  }

  & ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }
}
</style>
