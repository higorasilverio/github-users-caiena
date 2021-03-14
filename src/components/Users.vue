<template>
  <div class="users">
    <div style="margin: 0 25vw; border: 1px dashed blue;">
      <b-table v-if="users.length > 0" striped hover small :fields="fields" :items="users">
        <template #cell(userAvatar)="data">
          <b-img-lazy :src="data.value" v-bind="imgProps" alt="User avatar"></b-img-lazy>
        </template>
        <template #cell(userName)="data">
          <div style="height: 15vh !important; padding: 6vh 2vh;">
            <span>{{ data.value }}</span>
          </div>
        </template>
        <template #cell(userUrl)="data">
          <div style="height: 15vh !important; padding: 6vh 2vh;">
            <b-button block pill variant="outline-success" :href="data.value" target="_blank">Visit</b-button>
          </div>
        </template>
      </b-table>
    </div>
    <b-modal id="modal-get-access-token" centered hide-footer hide-header no-close-on-esc no-close-on-backdrop>
      <h5>Enter the access token:</h5>
      <b-input-group>
        <b-form-input type="password" placeholder="ACCESS TOKEN" v-model="token" @keypress.enter="take" />
        <b-input-group-append is-text>
          <b-icon icon="patch-check" @click="take"/>
        </b-input-group-append>
      </b-input-group>
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
        width: 100,
        height: 100
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
