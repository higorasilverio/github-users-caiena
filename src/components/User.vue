<template>
  <div class="wrapper">
  <div class="card">
    <div class="card-info">
      <div class="card-image">
        <img :src="userAvatar" class="card-avatar card-avatar--circle">
      </div>
      <div class="card-fields">
        <h5 class="card-name">{{ userName }}</h5>
      </div>
      <div class="card-fields">
        <b-button variant="outline-dark" :href="userUrl" target="_blank">
          <span class="card-icom"><b-icon icon="github" /></span><span class="card-span"> Profile</span>
        </b-button>
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
  max-width: 18.5em;
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
  display: flex;
  justify-content: center;
  margin: auto;
  max-width: ((128px/16px) * 1em);
}

.card-avatar {
  display: block;
  height: auto;
  width: 50%;
}

.card-avatar--circle {
  border-radius: 50%;
}

.card-name {
  margin-bottom: .2em;
  font-size: 1.5em;
}

.card-icom, .card-span {
  font-size: 1.3em;
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
  border-radius: .3em;
  margin: .5em;
  justify-content: center;
  height: 40vh;
  background: radial-gradient(circle, #ffffff, #abbaab);
}
@media (max-width: 990px){
  .card-avatar {
    width: 70%;
  }
  .card-name {
    font-size: 1.3em;
  }
}
@media (max-width: 550px){
  .card-avatar {
    width: 100%;
  }
  .card-span {
    display: none;
  }
  .card-icom {
    margin: 0 2vh;
  }
  .card-name {
    font-size: 1em;
  }
  .wrapper {
    margin: .1em;
  }
}
@media (max-width: 450px){
  .wrapper {
    height: 30vh;
  }
  .card-name {
    font-size: .8em;
  }
}
@media (max-width: 300px){
  .wrapper {
    height: 25vh;
  }
  .card-icom {
    margin: 0;
  }
  .card-avatar {
    width: 8vh;
  }
  .card-name {
    font-size: .60em;
  }
}
</style>
