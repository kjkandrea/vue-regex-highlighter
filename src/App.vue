<template>
  <div id="app">
    <form @submit.prevent="onSubmitForm()">
      <label>Regex : </label>
      <input type="text" v-model="query" @keydown="onChangeQuery" placeholder="ex) [1-9]\d" />
      <input type="submit" />
    </form>
    <article class="testArea">
      <div v-if="search" v-html="highlight()" />
      <div v-else>{{target}}</div>
    </article>
    <ul class="beforeSearchHistory">
      <li v-for="(item, index) in searchArray" :key="index">
        <button @click="onClickHistory(item)">{{item}}</button>
      </li>
    </ul>
    <button class="clearButton" @click="onClickClear()">Clear All</button>
  </div>
</template>

<script>
  
export default {
  name: 'App',
  data() {
    return {
      search: false,
      searchArray: [],
      query: "",
      target : 'Luke Skywarker 02-123-4567 luke@daum.net 다스베이더 070-9999-9999 darth_vader@gmail.com princess leia 010 2454 3457 leia@gmail.com'
    }
  },
  methods: {
    onSubmitForm() {
      this.searchArray.push(this.query)
      this.search = true
    },
    highlight() {
      if(!this.query) {
        return this.target;
      }

      try {
        return this.target.replace(new RegExp(this.query, "gi"), match => {
          return '<span class="highlightText">' + match + '</span>';
        });
      } catch(err) {
        // is Notting
      }
    },
    onClickClear() {
      this.searchArray = []
    },
    onClickHistory(query) {
      this.query = query
      this.search = true
    },
    onChangeQuery() {
      this.search = false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.testArea{
  max-width: 375px;
  margin: 20px auto;
  word-break: keep-all;
}

.highlightText {
  color: red;
}

.beforeSearchHistory, .beforeSearchHistory li {
  margin: 0;
  padding: 0;
  list-style: none;
}

.beforeSearchHistory button {
  margin: 2px;
}

.clearButton {
  display: block;
  margin: 20px auto 0;
  background: #fff;
}

</style>
