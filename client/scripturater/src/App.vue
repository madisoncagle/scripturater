<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup

export default {

  data() {
    return {
      allWords: {},
      word: {id: 0, word: "hebrews"},
      numberWordsRated: 0,
      wordsRated: []
    }
  },
  methods:{
    // probably won't use
    async getAllWords() {
      const result = await fetch("http://localhost:3000/api/words");
      const words = await result.json();

      this.allWords = words;
    },

    async loadNewWord() {
      const result = await fetch("http://localhost:3000/api/words/random");
      const word = await result.json();
      
      this.numberWordsRated++;
      //alert(word.word);

      this.word = word;
    },

    async submitRating(wordId, rating) {
      const result = await fetch(`http://localhost:3000/api/words/${wordId}/ratings`, 
      {
        method:"post", 
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify( {rating: rating} )
      });

      this.wordsRated.push({ word: this.word.word, rating: rating });
      // console.log(this.wordsRated);

      this.loadNewWord();
    },
  },
  mounted() {
    this.loadNewWord();
  }
}

</script>

<template>

  <div>
    <h1>Scripturater</h1>

    <h2>{{word.word.toUpperCase()}}</h2>

    <p>word #{{numberWordsRated}}</p>

    <a href="" class="like" @click.prevent="submitRating(word.id, 'like')"><span class="material-icons-round">sentiment_satisfied</span></a>
    <a href="" class="neutral" @click.prevent="submitRating(word.id, 'neutral')"><span class="material-icons-round">sentiment_neutral</span></a>
    <a href="" class="dislike" @click.prevent="submitRating(word.id, 'dislike')"><span class="material-icons-round">sentiment_dissatisfied</span></a>

    <div id="word-list">
      <p
      v-for="w in this.wordsRated"
      :key="w"
      :class="`${w.rating} muted`"
      >{{ w.word.toLowerCase() }}</p>
    </div>
    
    <!-- <br> -->
  
    <!-- <button id="newWordBtn" @click="loadNewWord">Get Another Word</button> -->
  </div>

</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap');

* {
  font-family: Poppins, Helvetica, Arial, sans-serif;
  background: #1d1d1d;
  box-sizing: border-box;
  /* border: 1px solid gray; */
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #a8c9e8;
  margin-top: 5%;
}

h2 {
  font-size: 3em;
  background-color: #353535;
  width: fit-content;
  padding: 0 20px 0 20px;
  margin: auto;
  border-radius: 20px;
}

a {
  font-size: 6em;
  text-decoration: none;
  color: #fff;
  margin: 2.5%;
}

p {
  color: #777;
}

.material-icons-round {
  font-size: 100%;
  background: transparent;
  transition: 0.15s;
}

.material-icons-round:hover {
  transform: scale(1.1);
}

.like {
  color: #538D4E;
}

.neutral {
  color: #B59F3B;
}

.dislike {
  color: #aa4d4d;
}

.muted {
  padding: 20px;
  margin: auto;
  opacity: 0.75;
}

#word-list {
  display: flex;
  flex-wrap: wrap;
}

/* #newWordBtn {
  margin-top: 1em;
  color: #fff;
  background: #505050;
} */
</style>
