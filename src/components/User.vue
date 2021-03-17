<template>
  <b-row class="user">
    <b-col cols="3"><b-img-lazy :src="userAvatar" v-bind="imgProps" alt="User avatar"></b-img-lazy></b-col>
		<b-col cols="8">
			<b-row><span>{{ userName }}</span></b-row>
			<b-row><a :href="userUrl" target="_blank">Visit Profile</a></b-row>
		</b-col>
  </b-row>
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
        width: 50,
        height: 50
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
