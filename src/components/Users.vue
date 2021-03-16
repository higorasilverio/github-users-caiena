<template>
  <div class="users">
    <b-modal id="modal-get-access-token" centered hide-footer hide-header no-close-on-esc no-close-on-backdrop>
      <h5>Enter the access token:</h5>
      <b-input-group>
        <b-form-input type="password" placeholder="ACCESS TOKEN" v-model="token" @keypress.enter="initiate" />
        <b-input-group-append is-text>
          <b-icon icon="patch-check" @click="initiate" />
        </b-input-group-append>
      </b-input-group>
    </b-modal>
    <b-modal id="modal-show-github-user" size="xl" centered hide-footer hide-header no-close-on-esc no-close-on-backdrop>
      <b-container fluid>
        <b-row>
          <b-input-group class="mb-5">
            <b-input-group-prepend is-text>
              <b-icon icon="search" @click="getUser" />
            </b-input-group-prepend>
            <b-form-input type="search" placeholder="Search user" v-model="user" @keypress.enter="getUser" />
          </b-input-group>
        </b-row>
        <b-row>
          <b-col cols="6" v-for="(user, index) in users" :key="index">
            <b-row>
              <b-col cols="2"><b-img-lazy :src="user.userAvatar" v-bind="imgProps" alt="User avatar"></b-img-lazy></b-col>
              <b-col cols="3">{{user.userName}}</b-col>
              <b-col><b-button block pill variant="outline-success" :href="user.userUrl" target="_blank">Visit</b-button></b-col>
            </b-row>
          </b-col>
        </b-row>
        <b-row class="mt-5">
          <label for="number">Page:</label>
          <b-form-spinbutton id="number" v-model="pageNumber" inline min="1" max="120"/>
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
      user: '',
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
      },
      pageNumber: 1
    }
  },
  mounted () {
    this.$bvModal.show('modal-get-access-token')
  },
  watch: {
    pageNumber: function () {
      this.changePage()
    }
  },
  methods: {
    initiate () {
      this.$bvModal.hide('modal-get-access-token')
      this.getUsers() ? this.$bvModal.show('modal-show-github-user') : this.$bvModal.show('modal-get-access-token')
    },
    changePage () {
      this.users.length = 0
      this.getUsers() ? this.$bvModal.show('modal-show-github-user') : this.pageNumber--
    },
    async getUsers () {
      await this.axios
        .get(`https://api.github.com/users?since=${this.pageNumber - 1}&per_page=20`, {
          headers: {
            Authorization: `token ${this.token}`,
            Accept: 'application/vnd.github.v3+json'
          }
        })
        .then(response => {
          response.data.forEach(element => {
            this.users.push({userAvatar: element.avatar_url, userName: element.login, userUrl: element.html_url})
          })
          return true
        })
        .catch(error => {
          console.error(error)
          return false
        })
    },
    async getUser () {
      await this.axios
        .get(`https://api.github.com/users/${this.user}`, {
          headers: {
            Authorization: `token ${this.token}`,
            Accept: 'application/vnd.github.v3+json'
          }
        })
        .then(response => {
          console.log(response)
        })
        .catch(error => {
          console.error(error)
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
