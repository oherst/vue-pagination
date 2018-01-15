<template>
  <div id="app">
    <h3>Paginated table test</h3>

    <button v-on:click="shuffle">Shuffle</button>

    <div class="container">
      <div>
        <div class="numbers" v-for="n in rowsToShow.length">{{ n }}</div>
      </div>

      <transition-group name="flip-list" tag="div">

        <table-row
          v-for="item in rowsToShow"
          :key="item.id"
          :item="item"
        >
        </table-row>
      </transition-group>

    </div>
    <div class="load-more" @click="addMore">
      Load more...
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';
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
    axios.get('https://jsonplaceholder.typicode.com/posts')
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
      const additional = partial.slice(curLen, curLen + 10);

      this.rowsToShow = [...this.rowsToShow, ...additional];
    },
    shuffle() {
      this.rowsToShow = _.shuffle(this.rowsToShow);
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  width: 70%;
  margin: auto;
}

.container {
  display: flex;
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

.load-more {
  margin: 10px;
  padding: 10px;
  background-color: #eee;
  &:hover {
    cursor: pointer;
  }
}
</style>
