<template>
  <div class="home">
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
        <b-row align-h="end">
          <b-col sm="6" md="6" lg="4" xl="3">
            <b-input-group class="mb-3">
              <b-input-group-prepend is-text>
                <b-icon icon="search" @click="getSingleUser" />
              </b-input-group-prepend>
              <b-form-input type="search" placeholder="Search user" v-model="user" @keypress.enter="getSingleUser" />
            </b-input-group>
          </b-col>
        </b-row>
        <b-row>
          <b-col cols="6" v-for="user in users" :key="user.userId">
            <User :userName="user.userName" :userAvatar="user.userAvatar" :userUrl="user.userUrl" />
          </b-col>
        </b-row>
        <b-row class="mt-2 mr-1" align-h="end">
          <div class="flex-pagination-div">
            <span>Page</span>
            <b-form-spinbutton id="number" v-model="pageNumber" inline min="1" max="120"/>
          </div>
        </b-row>
      </b-container>
    </b-modal>
    <b-modal id="modal-show-single-user" centered hide-footer hide-header no-close-on-esc no-close-on-backdrop>
      <b-container fluid>
        <b-row>
          <b-col>
            <SingleUser :userName="singleUser.userName" :userAvatar="singleUser.userAvatar" :userUrl="singleUser.userUrl" />
          </b-col>
        </b-row>
        <b-row align-h="center">
          <b-button variant="outline-dark" @click="closeSingleUser">
            <span class="button-return"><b-icon icon="arrow-return-left" /> Return</span>
          </b-button>
        </b-row>
      </b-container>
    </b-modal>
  </div>
</template>

<script>
import User from '../components/User'
import SingleUser from '../components/SingleUser'

export default {
  name: 'Home',
  components: {
    User,
    SingleUser
  },
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
          this.$bvModal.hide('modal-show-github-user')
          this.$bvModal.show('modal-show-single-user')
        })
        .catch(error => {
          console.error(error)
        })
    },
    closeSingleUser () {
      this.$bvModal.hide('modal-show-single-user')
      this.$bvModal.show('modal-show-github-user')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.flex-pagination-div {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.button-return {
  font-size: 1.3em;
  margin-bottom: .2em;
}
</style>
