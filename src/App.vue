<template>
  <div id="app">

    <div class="buttons">
      <button @click="shuffle">Move around</button>
      <button @click="addMore">Load more</button>
      <button @click="initArray">Reset</button>
    </div>

    <h3>Paginated table test</h3>
    <p>Showing {{ rowsToShow.length }} of {{ fakeData.length }}</p>

    <div class="container">
      <div class="numbers-container">
        <div class="numbers" v-for="n in rowsToShow.length">{{ n }}</div>
      </div>

      <transition-group name="flip-list" tag="div" class="rows-container">

        <table-row
          v-for="item in rowsToShow"
          :key="item.id"
          :item="item"
        >
        </table-row>
      </transition-group>

    </div>
  </div>
</template>

<script>
import axios from 'axios';
import TableRow from './table-row.vue';

export default {
  name: 'app',
  data () {
    return {
      fakeData: [],
      rowsToShow: []
    }
  },
  components: {
    TableRow
  },

  created() {
    axios.get('https://jsonplaceholder.typicode.com/comments')
    .then((response) => {
      this.fakeData = response.data;
    })
    .then(this.initArray);
  },

  methods: {
    initArray() {
      const [...partial] = this.fakeData;
      this.rowsToShow = partial.slice(0,10);
    },
    addMore() {
      const curLen = this.rowsToShow.length;
      const [...partial] = this.fakeData;
      const additional = partial.slice(curLen, curLen + 20);

      this.rowsToShow = [...this.rowsToShow, ...additional];
    },
    randomNum() {
      const max = this.rowsToShow.length;
      return Math.floor(Math.random() * max);
    },
    shuffle() {
      const [...items] = this.rowsToShow;
      const idx = this.randomNum();
      const randomItem = items[idx];

      items.splice(idx, 1);
      items.splice(this.randomNum(), 0, randomItem);

      this.rowsToShow = items;
    }
  }
}
</script>

<style lang="scss">
#app {
  position: relative;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  width: 70%;
  margin: auto;
}

.container {
  display: flex;
  margin-top: 50px;
  .row:nth-child(odd) {
    background-color: #eee;
  }
}

.numbers {
  width: 50px;
}

.numbers,
.row {
  height: 30px;
}

.flip-list-move {
  transition: transform 1s;
}

.buttons {
  position: absolute;
  display: flex;
  flex-direction: column;
  right: 0;
  top: 0;
  button {
    margin-bottom: 5px;
  }
}

.rows-container {
  flex: 1 0 auto;
}
</style>
