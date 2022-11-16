<template>
  <div>
    <div class="form-group">
      <input type="text" placeholder="Mots clés..." class="form-control form-control" v-model="search">
      <button type="button" class="btn btn-primary" @click.prevent="() => {this.page=1;searchWord()}">Chercher des blagues</button>
    </div>

    <div class="error" v-if="error">Pas de résultat !</div>

    <section v-if="result.length > 0">
      <header id="pages">
        <button type="button" class="btn btn-secondary" @click.prevent="changePage(false)" :disabled="(this.page === 1)"> prev</button>
        <div>{{ page }}</div>
        <div>/</div>
        <div>{{ total_pages }}</div>
        <button type="button" class="btn btn-secondary" @click.prevent="changePage(true)" :disabled="(this.page === this.total_pages)"> next</button>
      </header>

      <JokeComponent v-for="(item, number) in this.result" :key="number" :joke="item.joke"/>
      <footer style="justify-content: left;"><div style="margin-right: 1em; font-style: italic;">Mots clés :</div> "<div id="keywords">{{searchKeyword}}</div>" </footer>
    </section>



  </div>
</template>

<script>


import axios from "axios";
import JokeComponent from "@/components/JokeComponent";

export default {
  name: 'App',
  components: {JokeComponent},
  watch: {
    page() {
      this.searchWord()
    }
  },
  methods: {
    searchWord() {
      axios.get('https://icanhazdadjoke.com/search?term=' + this.search + '&page=' + this.page + '&limit=5', {
        headers: {
          "Accept": 'application/json'
        }
      }).then((res) => {
        let data = res.data;
        this.result = data.results;
        this.total_pages = data.total_pages;
        this.searchKeyword = data.search_term;

        this.error = this.result.length === 0;
      }).catch(() => {
        console.error('error')
      })
    },
    changePage(isIncrease) {
      if(isIncrease && this.page+1 <= this.total_pages) {
        this.page++;
      } else if(!isIncrease && this.page-1 > 0) {
        this.page--;
      }
    }
  },
  data() {
    return {
      search: '',
      result: [],
      total_pages: 0,
      page: 1,
      searchKeyword: '',
      error:false
    }
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.form-group {
  display: flex;
  margin: 5em 1em;
}

.form-group > input {
  margin: 0 10px;
}

.form-group > button {
  flex-shrink: 0;
}
section {
  width: 90%;
  margin: 0 auto;
}
section > header,footer {
  padding: 5px;
  background: #fdf5e6;
  display: flex;
  justify-content: center;
  align-items: center;
}
.error {
  background: #ffb6c1;
  padding: 10px;
  margin: 0 auto;
  width: 90%;
}
</style>
