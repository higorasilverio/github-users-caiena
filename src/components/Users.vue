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
      <b-container fluid v-if="showUserList">
        <b-row>
          <b-input-group class="mb-5">
            <b-input-group-prepend is-text>
              <b-icon icon="search" @click="getSingleUser" />
            </b-input-group-prepend>
            <b-form-input type="search" placeholder="Search user" v-model="user" @keypress.enter="getSingleUser" />
          </b-input-group>
        </b-row>
        <b-row>
          <b-col cols="6" v-for="user in users" :key="user.userId">
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
      <b-container fluid v-else>
        <b-row>
          <b-col cols="2"><b-img-lazy :src="singleUser.userAvatar" v-bind="imgProps" alt="User avatar"></b-img-lazy></b-col>
          <b-col cols="3">{{singleUser.userName}}</b-col>
          <b-col><b-button block pill variant="outline-success" :href="singleUser.userUrl" target="_blank">Visit</b-button></b-col>
        </b-row>
        <b-row>
          <b-button block pill variant="outline-success" @click="showUserList = true">Return</b-button>
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
      singleUser: {userId: 0, userAvatar: '', userName: '', userUrl: ''},
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
      pageNumber: 1,
      showUserList: true
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
        .get(`https://api.github.com/users?since=${(this.pageNumber - 1) * 20}&per_page=20`, {
          headers: {
            Authorization: `token ${this.token}`,
            Accept: 'application/vnd.github.v3+json'
          }
        })
        .then(response => {
          response.data.forEach(element => {
            this.users.push({userId: element.id, userAvatar: element.avatar_url, userName: element.login, userUrl: element.html_url})
          })
          return true
        })
        .catch(error => {
          console.error(error)
          return false
        })
    },
    async getSingleUser () {
      await this.axios
        .get(`https://api.github.com/users/${this.user}`, {
          headers: {
            Authorization: `token ${this.token}`,
            Accept: 'application/vnd.github.v3+json'
          }
        })
        .then(response => {
          this.singleUser = {
            userId: response.data.id,
            userAvatar: response.data.avatar_url,
            userName: response.data.name,
            userUrl: response.data.html_url
          }
          this.showUserList = false
          console.log(this.singleUser)
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
