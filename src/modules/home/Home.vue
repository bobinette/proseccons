<template>
  <div>
    <navbar></navbar>
    <div class="container">
      <!-- Proseccons -->
      <pc-table
        v-for="(proseccon, idx) in proseccons"
        class="ProsecconTable"
        :key="idx"
        :title="proseccon.title"
        :pros="proseccon.pros"
        :cons="proseccon.cons"
        @new-item="(item, key) => newItem(idx, item, key)"
      ></pc-table>

      <!-- New proseccon input -->
      <input
        type="text"
        class="ProsecconInput"
        v-model="title"
        @keyup.13="newProseccon"
        placeholder="Title of the new Proseccon, e.g. &quot;Should I buy that fancy Millenium Falcon?&quot;"
      >
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/navbar/Navbar';
import Table from './table/Table';

export default {
  name: 'Home',
  data() {
    return {
      title: '',
      proseccons: [],
    };
  },
  methods: {
    newProseccon() {
      this.proseccons.push({ title: this.title, pros: [], cons: [] });
      this.title = '';
    },
    newItem(idx, item, key) {
      console.log(idx, item, key);
      this.proseccons[idx][key].push(item);
    },
  },
  watch: {
    proseccons: {
      handler() {
        localStorage.setItem('proseccons', JSON.stringify(this.proseccons));
      },
      deep: true,
    },
  },
  components: {
    'pc-table': Table, // Because table is reserved
    Navbar,
  },
  mounted() {
    const proseccons = JSON.parse(localStorage.getItem('proseccons'));
    if (proseccons) {
      this.proseccons = proseccons;
    }
  },
};
</script>

<style>
.ProsecconTable {
  margin-bottom: 2rem;
}

.ProsecconInput {
  width: 100%;
}
</style>
