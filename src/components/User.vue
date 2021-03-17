<template>
  <div class="wrapper">
  <div id="card" class="card">
    <div class="card-info">
      <div class="card-image">
        <img :src="userAvatar" class="card-avatar card-avatar--circle">
      </div>
      <div class="card-fields">
        <h3 class="card-name">{{ userName }}</h3>
      </div>
      <div class="card-fields">
        <b-button :href="userUrl" target="_blank"><span class="card-icom"><b-icon icon="github"></b-icon></span></b-button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'User',
  props: ['userAvatar', 'userName', 'userUrl']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  align-items: center;
  background: #fff;
  border-radius: .3em;
  border: .0625em solid mix(#fff, #abbaab, 20%);
  box-shadow: 0 0 1.7em 0 rgba(#abbaab, .55);
  display: flex;
  flex-direction: column;
  max-width: 18em;
  position: relative;
  width: 100%;
}

.card-info {
  padding: 1.2em 1em;
  position: relative;
  text-align: center;
  width: 100%;
}

.card-image {
  margin: auto;
  max-width: ((128px/16px) * 1em);
}

.card-avatar {
  display: block;
  height: auto;
  width: 100%;
}

.card-avatar--circle {
  border-radius: 50%;
}

.card-name {
  font-size: 1.8em;
  margin-bottom: .2em;
}

.card-icom {
  font-size: 1.5em;
  margin-bottom: .2em;
}

.card-fields {
  display: flex;
  justify-content: center;
}

html {
  box-sizing: border-box;
}

*, *::before, *::after {
  box-sizing: inherit;
}

.wrapper {
  align-items: center;
  display: flex;
  justify-content: center;
  height: 55vh;
  background: radial-gradient(circle, #ffffff, #abbaab);
}
</style>
