<template>
  <div>
    <ul v-for="word in words">
      <li>{{ word }}</li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Last25StoriesPage',

  data() {
    return {
      url: {
        base: 'https://hacker-news.firebaseio.com/v0/',
        newStories: 'newstories.json',
        singleStory: '/item/{id}.json',
      },
      stories: [],
      story: '',
      words: ''
    }
  },

  methods: {
    async getStories() {
      const { data } = await axios.get(this.url.base + this.url.newStories);
      this.stories = data;
      this.getStory();
    },

    getStory() {
      for (let i = 0; i < 25; i++) {
        axios
          .get(this.url.base + this.url.singleStory.replace('{id}', this.stories[i]))
          .then(response => (this.story += response.data.title + ' '))
          .catch(error => console.log(error))
          .then(() => {
            this.findTopTenWords()
          });
      }
    },

    findTopTenWords() {
      this.words = this.story
        .replace(/[^\w\s]|_/g, "")
        .replace(/\s+/g, " ")
        .toLowerCase()
        .split(' ');

      const mappings = {}

      for (let i = 0; i < this.words.length; i++) {
        mappings[this.words[i]] = mappings[this.words[i]] + 1 || 1
      }

      const sorted = Object.keys(mappings).sort((a, b) => {
        if (mappings[b] === mappings[a]) {
          return a > b ? 1 : -1
        }
        return mappings[b] - mappings[a]
      })

      this.words = sorted.slice(0, 10)
    }
  },

  created() {
    this.getStories();
  },
}
</script>

<style>
</style>