<template>
  <div class="users">
    <b-modal id="modal-get-access-token" centered hide-footer hide-header no-close-on-esc no-close-on-backdrop>
      <h5>Enter the access token:</h5>
      <b-input-group>
        <b-form-input type="password" placeholder="ACCESS TOKEN" v-model="token" @keypress.enter="take" />
        <b-input-group-append is-text>
          <b-icon icon="patch-check" @click="take"/>
        </b-input-group-append>
      </b-input-group>
    </b-modal>
    <b-modal id="modal-show-github-user" size="xl" centered hide-footer hide-header no-close-on-esc no-close-on-backdrop>
      <b-container fluid>
        <b-row>
          <b-col cols="6" v-for="user in users" :key="user.userName">
            <b-row>
              <b-col cols="2"><b-img-lazy :src="user.userAvatar" v-bind="imgProps" alt="User avatar"></b-img-lazy></b-col>
              <b-col cols="3">{{user.userName}}</b-col>
              <b-col><b-button block pill variant="outline-success" :href="user.userUrl" target="_blank">Visit</b-button></b-col>
            </b-row>
          </b-col>
        </b-row>
      </b-container>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Users',
  data () {
    return {
      token: '',
      users: [],
      fields: [
        { key: 'userAvatar', label: '' },
        { key: 'userName', label: 'User' },
        { key: 'userUrl', label: "User's profile" }
      ],
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
  mounted () {
    this.$bvModal.show('modal-get-access-token')
  },
  methods: {
    take () {
      this.$bvModal.hide('modal-get-access-token')
      this.axios
        .get('https://api.github.com/users?page=1&per_page=20', {
          headers: {
            Authorization: `token ${this.token}`
          }
        })
        .then(res => {
          res.data.forEach(element => {
            this.users.push({userAvatar: element.avatar_url, userName: element.login, userUrl: element.html_url})
          })
          this.$bvModal.show('modal-show-github-user')
        })
        .catch(error => {
          console.error(error)
          this.$bvModal.show('modal-get-access-token')
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
</style>
