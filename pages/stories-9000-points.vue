<template>
  <div>
    9000
  </div>
</template>

<script>
export default {
  name: 'Stories9000PointsPage',

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
}
</script>

<style>
</style>