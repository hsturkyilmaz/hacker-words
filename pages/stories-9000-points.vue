<template>
  <div class="container">
    <div class="list">
      <a href="https://news.ycombinator.com/newest" target="_blank">
        <div class="list__title">
          Last 25 Stories
          <img :src="ArrowIcon" class="list__icon" />
        </div>
      </a>
      <div v-if="!this.words" class="list__loading">
        <div class="border">
          <div class="progress">
          </div>
        </div>
      </div>
      <div v-for="(word, index) in words" :key="index" class="list__item">
        {{ word }}
      </div>
    </div>
    <a href="/">
      <img :src="BackIcon" class="back-icon" />
    </a>
  </div>
</template>
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
      ArrowIcon: require("../assets/arrow-icon.svg"),
      BackIcon: require("../assets/back-icon.svg"),
      numberOfStories: 25,
      stories: [],
      words: '',
      user: '',
      title: '',
      filteredUsers: '',
    }
  },

  methods: {
    async getStories() {
      const { data } = await axios.get(this.url.base + this.url.newStories);
      this.stories = data;
      this.getFilteredUsers();
    },

    async getFilteredUsers() {
      for (let i = 0; i < this.numberOfStories; i++) {
        await axios
          .get(this.url.base + this.url.singleStory.replace('{id}', this.stories[i]))
          .then(response => (this.user += response.data.by + ' '))
          .catch(error => console.log(error))
          .then(() => {
            axios
              .get(this.url.base + this.url.user.replace('{id}', this.user.split(' ')[i]))
              .then(response => {
                if (response.data.karma >= 9000) {
                  this.filteredUsers += response.data.id + ' '
                }
              })
              .catch(error => console.log(error))
          });
      }
      this.getTitles();
    },

    getTitles() {
      for (let i = 0; i < this.numberOfStories; i++) {
        axios
          .get(this.url.base + this.url.singleStory.replace('{id}', this.stories[i]))
          .then(response => {
            this.filteredUsers.includes(response.data.by) ? this.title += response.data.title + ' ' : ''
          })
          .then(() => {
            this.findTopTenWords()
          })
      }
    },

    findTopTenWords() {
      this.words = this.title
        .replace(/[^\w\s]|_/g, "")
        .replace(/\s+/g, " ")
        .toLowerCase()
        .replace(/[0-9]/g, '')
        .split(' ')
        .filter(word => word !== '')
        .filter(word => word.replace(/\s+/g, '').length > 1)

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

<style lang="scss" scoped>
.container {
  width: 100vw;
  height: 100vh;
  background: #F1EFDC;
  display: flex;
  justify-content: center;
  align-items: center;
}

.list {
  width: 40vw;
  height: 20rem;
  background-color: #E6D2AA;
  border-radius: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  font-size: 2rem;
  position: relative;

  &__title {
    background-color: #D36B00;
    padding: 0.25rem 0.5rem;
    border-radius: 0.5rem;
    color: #fff;
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0.8;
  }

  &__item {
    background: var(--item-color);

    @for $i from 0 through 30 {
      &:nth-child(#{$i}) {
        --item-color: #{rgba(random(255), random(255), random(255), 0.4)};
      }
    }

    color: #42032C;
    padding: 0.25rem 0.5rem;
    border-radius: 0.5rem;
  }

  &__icon {
    width: 1.5rem;
    height: 1.5rem;
  }
}

.back-icon {
  width: 3rem;
  height: 3rem;
  position: absolute;
  top: 0;
  left: 0;
  margin: 1rem;
}

.border {
  width: 300px;
  border-color: #F1EFDC;
  border-style: solid;
  border-radius: 15px;
}

.progress {
  background: linear-gradient(to right, #ffff00, #00ff00);
  height: 14px;
  border-style: solid;
  border-radius: 15px;
  border-color: #F1EFDC;
  animation: width 3s ease-out infinite;
}

@keyframes width {
  from {
    width: 0px
  }

  to {
    width: 294px
  }
}
</style>