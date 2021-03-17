<template>
  <div class="user">
    <b-col cols="2"><b-img-lazy :src="userAvatar" v-bind="imgProps" alt="User avatar" /></b-col>
    <b-col cols="3">{{ userName }}</b-col>
    <b-col><b-button block pill variant="outline-success" :href="userUrl" target="_blank" >Visit</b-button></b-col>
  </div>
</template>

<script>
export default {
  name: 'User',
  props: ['userAvatar', 'userName', 'userUrl'],
  data () {
    return {
      imgProps: {
        left: true,
        fluid: true,
        blank: true,
        rounded: 'circle',
        blankColor: '#fff',
        width: 40,
        height: 40
      }
    }
  },
  methods: {
    initiate () {
      console.log('User initiated')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
