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
        @new-item="(input) => newItem(idx, input)"
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

const re = new RegExp(/^(.*)(?:(?:: )(.*)) ([-+]\d)$/);

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
  name: 'Home',
  data() {
    return {
      title: '',
      proseccons: [
        {
          title: 'First PC',
          pros: [
            {
              title: 'funny',
              description:
                'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
            },
            {
              title: 'new',
              description:
                'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
            },
          ],
          cons: [
            {
              title: 'learning curve',
              description:
                'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
            },
          ],
        },
      ],
    };
  },
  methods: {
    newProseccon() {
      this.proseccons.push({ title: this.title, pros: [], cons: [] });
      this.title = '';
    },
    newItem(idx, input) {
      const proseccon = parseItem(input);
      if (!proseccon || proseccon.score === 0) {
        return;
      }

      if (proseccon.score > 0) {
        this.proseccons[idx].pros.push(proseccon);
      } else {
        proseccon.score = -proseccon.score;
        this.proseccons[idx].cons.push(proseccon);
      }
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
