<template>
  <div class="container">
    <div class="menu">
      <div class="menu__description">
        <p>The 10 most common words in the last 25 story titles on Hacker News</p>
      </div>
      <div class="menu__button">
        <a href="/last-25-stories">
          <button>Show All</button>
        </a>
        <a href="/stories-9000-points">
          <button>Show 9000+ Points</button>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NavigationMenu'
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

.menu {
  width: 40vw;
  height: 20rem;
  background-color: #E6D2AA;
  border-radius: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2rem;

  &__description {
    display: flex;
    gap: 2rem;
    width: 50%;
    text-align: center;
    line-height: 1.5rem;
  }

  &__button {
    display: flex;
    gap: 2rem;

    button {
      padding: 1rem 3rem;
      background: #D36B00;
      color: #FFF;
      border: none;
      border-radius: 0.5rem;
      font-size: 1rem;
      cursor: pointer;

      &:hover {
        background: #42032C;
      }
    }
  }
}
</style>