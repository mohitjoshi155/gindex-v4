<template>
  <div>
    <div v-if="admin" class="content mt-5">
      <h1 class="title has-text-centered has-text-weight-bold has-text-white">Your Admin Zone</h1>
      <div class="loading">
        <loading :active.sync="loading" :can-cancel="false" :is-full-page="fullpage"></loading>
      </div>
      <div class="columns has-text-white is-centered is-multiline">
        <div :class=" notify ? 'column is-full' : 'column is-hidden is-full' ">
          <div class="notification has-text-centered is-danger">
            <button class="delete" @click="notify = false"></button>
            <h2 class="title has-text-white has-text-weight-bold">Disclaimer !!</h2>
            <p class="subtitle has-text-weight-bold">Use your Admin Powers Wisely !!.</p>
          </div>
        </div>
        <div class="column is-two-fifths">
          <h2 class="title has-text-primary has-text-centered has-text-weight-bold">Permissions</h2>
          <p class="subtitle has-text-primary-dark has-text-centered has-text-weight-medium">You Have Given Following Scopes of Permission in this Website</p>
          <div class="columns is-multiline is-desktop is-centered">
            <div class="column is-full">
              <div class="box has-background-warning">
                <h2 class="title has-text-centered has-text-weight-bold">Related to New Users</h2>
                <div class="columns is-vcentered is-centered is-multiline is-mobile">
                  <div class="column is-two-thirds">
                    <p class="subtitle">Add a User</p>
                  </div>
                  <div class="column is-one-third">
                    <button class="button is-rounded is-light" @click="gotoPage('/0:register/')">
                      <span class="icon is-small">
                        <i class="fas fa-user-plus"></i>
                      </span>
                      <span>Add</span>
                    </button>
                  </div>
                  <div class="column is-two-thirds">
                    <p class="subtitle">Invite Users</p>
                  </div>
                  <div class="column is-one-third">
                    <button class="button is-rounded is-light" @click="gotoPage('/0:invite/')">
                      <span class="icon is-small">
                        <i class="fas fa-user-plus"></i>
                      </span>
                      <span>Invite</span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <div v-if="admin && superadmin" class="column is-full">
              <div class="box has-background-primary">
                <h2 class="title has-text-centered has-text-weight-bold">Permission to Users</h2>
                <div class="columns is-vcentered is-centered is-multiline is-mobile">
                  <div class="column is-two-thirds">
                    <p class="subtitle">Change Permission of Users</p>
                  </div>
                  <div class="column is-one-third">
                    <button class="button is-rounded is-black" @click="gotoPage('/0:register/')">
                      <span class="icon is-small">
                        <i class="fas fa-user-shield"></i>
                      </span>
                      <span>Promote</span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <div class="column is-full">
              <div class="box has-background-danger">
                <h2 class="title has-text-centered has-text-white has-text-weight-bold">Related to Existing Users</h2>
                <div class="columns is-vcentered is-centered is-multiline is-mobile">
                  <div class="column is-two-thirds">
                    <p class="subtitle has-text-white">Delete Users</p>
                  </div>
                  <div class="column is-one-third">
                    <button class="button is-rounded is-warning" @click="gotoPage('/0:delete/')">
                      <span class="icon is-small">
                        <i class="fas fa-user-times"></i>
                      </span>
                      <span>Delete</span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="column is-three-fifths">
          <div class="columns is-multiline is-desktop is-centered">
            <div class="column is-full">
              <div class="box has-background-light">
                <h3 class="title has-text-centered has-text-weight-bold has-text-info-dark">Admin Details</h3>
                <div class="columns is-vcentered is-multiline is-mobile">
                  <div class="column is-three-fifths">
                    <p class="subtitle">Name</p>
                  </div>
                  <div class="column is-two-fifths">
                    <p class="subtitle has-text-weight-bold">{{ user.name }}</p>
                  </div>
                  <div class="column is-three-fifths">
                    <p class="subtitle">Email</p>
                  </div>
                  <div class="column is-two-fifths">
                    <p class="subtitle has-text-weight-bold">{{ user.email }}</p>
                  </div>
                  <div class="column is-three-fifths">
                    <p class="subtitle">Current Role</p>
                  </div>
                  <div class="column is-two-fifths">
                    <p class="subtitle has-text-weight-bold">{{ user.role }}</p>
                  </div>
                  <div v-if="admin && !superadmin" class="column is-three-fifths">
                    <p class="subtitle">Request Superadmin Status for More Powers</p>
                  </div>
                  <div v-if="admin && !superadmin" class="column is-two-fifths">
                    <button class="button is-success" @click="gotoPage('/0:settings/request/')">
                      <span class="icon is-small">
                        <i class="fas fa-user-shield"></i>
                      </span>
                      <span>Request</span>
                    </button>
                  </div>
                  <div v-if="admin && superadmin" class="column has-text-centered is-full">
                    <p class="subtitle has-text-warning-dark has-text-weight-bold">You Have Maximum Access to this Website</p>
                  </div>
                  <div class="column is-three-fifths">
                    <p class="subtitle">Last Token Issue Date</p>
                  </div>
                  <div class="column is-two-fifths">
                    <p class="subtitle has-text-weight-bold">{{ token.issuedate | moment("dddd, MMMM Do YYYY [at] hh:mm A") }}</p>
                  </div>
                  <div class="column is-three-fifths">
                    <p class="subtitle">Last Token Expiry Date</p>
                  </div>
                  <div class="column is-two-fifths">
                    <p class="subtitle has-text-weight-bold">{{ token.expirydate | moment("dddd, MMMM Do YYYY [at] hh:mm A") }}</p>
                  </div>
                </div>
              </div>
            </div>
            <div class="column is-full">
              <div class="box has-background-info">
                <h3 class="title has-text-centered has-text-weight-bold has-text-white">Personal</h3>
                <p class="subtitle has-text-light has-text-centered has-text-weight-medium">Change Your Preferences</p>
                <div class="columns is-vcentered is-centered is-multiline is-mobile">
                  <div class="column is-9">
                    <p class="subtitle has-text-light">Go to My Settings</p>
                  </div>
                  <div class="column is-3">
                    <button class="button is-rounded is-light" @click="gotoPage('/0:settings/')">
                      <span class="icon is-small">
                        <i class="fas fa-user-cog"></i>
                      </span>
                      <span class="is-hidden-mobile">Settings</span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/vue-loading.css';
    export default {
        components: {
          Loading
        },
        data() {
          return {
            notify: true,
            user: {},
            token: {},
            admin: false,
            superadmin: false,
            loading: false,
            fullpage: true,
          }
        },
        methods: {
          gotoPage: function(url){
            this.$router.push(url)
          }
        },
        beforeMount() {
          this.loading = true;
          var token = localStorage.getItem("tokendata")
          var user = localStorage.getItem("userdata");
          if (user != null && token != null){
            var tokenData = JSON.parse(this.$hash.AES.decrypt(token, this.$pass).toString(this.$hash.enc.Utf8));
            var userData = JSON.parse(this.$hash.AES.decrypt(user, this.$pass).toString(this.$hash.enc.Utf8));
            this.user = userData, this.token = tokenData, this.loading = false;
          } else {
            this.user = null, this.token = null, this.loading = false;
          }
        },
        mounted() {
          if(this.user.admin && this.user.superadmin){
            this.admin = true, this.superadmin = true,this.loading = false;
          } else if(this.user.admin && !this.user.superadmin) {
            this.admin = true, this.superadmin = false,this.loading = false;
          } else {
            this.$router.push({ name: 'results', params: { id: 0, cmd: "result", data: "UnAuthorized Route.", redirectUrl: "/0:home/" } })
          }
        },
      }
</script>
