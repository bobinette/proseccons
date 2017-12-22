<template>
  <div>
    <navbar></navbar>
    <div class="container">
      <pc-table
        :title="title"
        :pros="pros"
        :cons="cons"
        @new-proseccon="newProseccon"
      ></pc-table>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/navbar/Navbar';
import Table from './table/Table';

const re = new RegExp(/^(.*)(?:(?:: )(.*)) ([-+]\d)$/);

const parseProseccon = (input) => {
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
    };
  },
  methods: {
    newProseccon(input) {
      const proseccon = parseProseccon(input);
      if (!proseccon || proseccon.score === 0) {
        return;
      }

      if (proseccon.score > 0) {
        this.pros.push(proseccon);
      } else {
        proseccon.score = -proseccon.score;
        this.cons.push(proseccon);
      }
    },
  },
  components: {
    'pc-table': Table, // Because table is reserved
    Navbar,
  },
};
</script>

<style>

</style>
