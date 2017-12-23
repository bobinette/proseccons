<template>
  <div class="card Proseccon">
    <div class="flex card-header flex-align-center flex-space-between" :class="[{ collapsed: !expanded }]">
      <h2>{{ title }}</h2>
      <div>
        <span class="success">+{{ proScore }}</span> / <span class="danger">-{{ conScore }}</span>
        <button class="btn btn-link" @click="() => { this.expanded = !this.expanded }">
          <i v-if="expanded" class="fa fa-angle-up"></i>
          <i v-else class="fa fa-angle-down"></i>
        </button>
      </div>
    </div>
    <div class="container Table" v-if="expanded">
      <!-- Pros and cons columns -->
      <div class="row">
        <div class="col-md-6 Column">
          <ul>
            <cell v-for="(item, idx) in pros" :key="idx" :item="item" score-style="success"></cell>
          </ul>
          <input type="text" v-model="proInput" @keyup.13="addItem(proInput, 'pros')" placeholder="Add a new pro in the format &quot;title: description ±1&quot;">
        </div>
        <div class="col-md-6 Column">
          <ul>
            <cell v-for="(item, idx) in cons" :item="item" :key="idx" score-style="danger"></cell>
          </ul>
          <input type="text" v-model="conInput" @keyup.13="addItem(conInput, 'cons')" placeholder="Add a new con in the format &quot;title: description ±1&quot;">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Cell from './cell/Cell';

const re = new RegExp(/^(.*)(?:(?:: )(.*)) (\d)$/);

const parseItem = (input) => {
  const matches = re.exec(input);
  if (!matches) {
    return null;
  }

  return {
    title: matches[1],
    description: matches[2],
    score: parseInt(matches[3], 10),
  };
};

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
      proInput: '',
      conInput: '',
      expanded: false,
    };
  },
  computed: {
    proScore() {
      return this.pros.reduce((acc, pro) => acc + (pro.score || 1), 0);
    },
    conScore() {
      return this.cons.reduce((acc, con) => acc + (con.score || 1), 0);
    },
  },
  methods: {
    addItem(input, key) {
      const item = parseItem(input);
      if (!item) {
        return;
      }

      this.$emit('new-item', item, key);

      if (key === 'pros') {
        this.proInput = '';
      } else {
        this.conInput = '';
      }
    },
  },
  components: {
    Cell,
  },
};
</script>

<style scoped lang="scss">
@import 'style/_variables.scss';

.Proseccon {
  h2 {
    margin: 0;
  }
}

.card-header.collapsed {
  border-bottom: none;
  border-radius: calc(0.25rem - 1px) calc(0.25rem - 1px);
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

input {
  margin: 1rem;
}
</style>
