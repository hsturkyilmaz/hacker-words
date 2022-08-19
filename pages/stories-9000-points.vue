<template>
  <div>
    filteredUsers: {{ filteredUsers }}
    <hr>
    title: {{ title }}
    <hr>
    user: {{ user }}
    <hr>
    words: {{ words }}
    <hr>
    story: {{ story }}
    <hr>
    karma: {{ karma }}
    <hr>
    stories: {{ stories }}
    <hr>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Stories9000PointsPage',

  data() {
    return {
      url: {
        base: 'https://hacker-news.firebaseio.com/v0/',
        newStories: 'newstories.json',
        singleStory: 'item/{id}.json',
        user: 'user/{id}.json',
      },
      stories: [],
      story: '',
      words: '',
      user: '',
      karma: '',
      title: '',
      filteredUsers: '',
    }
  },

  methods: {
    async getStories() {
      const { data } = await axios.get(this.url.base + this.url.newStories);
      this.stories = data;
      this.getKarma();
    },

    getKarma() {
      for (let i = 0; i < 50; i++) {
        axios
          .get(this.url.base + this.url.singleStory.replace('{id}', this.stories[i]))
          .then(response => (this.user += response.data.by + ' '))
          .catch(error => console.log(error))
          .then(() => {
            axios
              .get(this.url.base + this.url.user.replace('{id}', this.user.split(' ')[i]))
              .then(response => {
                if (response.data.karma >= 9000) {
                  console.log("this user: ", response.data.id)
                  this.filteredUsers = response.data.id
                }
              })
              .catch(error => console.log(error))
              .then(() => {
                axios
                  .get(this.url.base + this.url.singleStory.replace('{id}', this.stories[i]))
                  .then(response => {
                    this.filteredUsers.includes(response.data.by) ? this.story += response.data.title + ' ' : null
                  })
              });
          });

      }
    },
  },

  created() {
    this.getStories();
  },

  watch: {
    story: function () {
      console.log("story: ", this.story)
    }
  }
}
</script>

<style>
</style>