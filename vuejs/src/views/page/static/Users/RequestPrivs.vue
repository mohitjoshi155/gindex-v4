<template>
    <div class="content is-clipped">
        <div class="loading">
          <loading :active.sync="loading" :can-cancel="false" :is-full-page="fullpage"></loading>
        </div>
        <div class="columns is-vcentered is-centered is-multiline">
          <div class="column is-half has-text-centered has-text-white">
            <div :class=" adminmodal ? 'modal is-active' : 'modal' ">
              <div class="modal-background"></div>
              <div class="modal-card">
                <header class="modal-card-head">
                  <p class="modal-card-title has-text-centered">Admin Features</p>
                  <button class="delete" @click="adminmodal = false" aria-label="close"></button>
                </header>
                <section class="modal-card-body">
                  <div class="columns is-multiline is-desktop">
                    <div class="column is-full">
                      <p class="subtitle has-text-weight-bold has-text-centered">Features</p>
                    </div>
                    <div class="column is-full">
                      <p class="subtitle has-text-weight-semibold">Permissions Related to New Users:</p>
                    </div>
                    <div class="column is-full">
                      <p class="subtitle">Following Permissions are Related to New Users</p>
                    </div>
                    <div class="column is-full">
                      <ul>
                        <li> Accept New Users</li>
                        <li> Invite New Users through Glory to Heaven Mail Service.</li>
                      </ul>
                    </div>
                    <div class="column is-full">
                      <p class="subtitle has-text-weight-semibold">Permission Related to Existing Users:</p>
                    </div>
                    <div class="column is-full">
                      <p class="subtitle">Following Permissions are Related to Handling of Existing Users</p>
                    </div>
                    <div class="column is-full">
                      <ul>
                        <li> Delete a User</li>
                        <li> Add a User to Spam</li>
                      </ul>
                    </div>
                  </div>
                </section>
              </div>
            </div>
            <article :class=" errorMessage ? 'message is-danger' : 'message is-hidden is-danger'">
              <div class="message-header">
                <p>Error Logging in!!</p>
                <button class="delete" @click="errorMessage = false" aria-label="delete"></button>
              </div>
              <div class="message-body">
                {{ resultmessage }}
              </div>
            </article>
            <article :class=" successMessage ? 'message is-success' : 'message is-hidden is-success'">
              <div class="message-header">
                <p>Success !</p>
                <button class="delete" @click="successMessage = false" aria-label="delete"></button>
              </div>
              <div class="message-body">
                {{ resultmessage }}
              </div>
            </article>
            <h2 class="title has-text-weight-bold has-text-white">Request Access</h2>
            <form @submit.prevent="handleSubmit">
              <div class="control mb-3">
                  <input class="is-checkradio is-small is-warning" id="adminradio" type="radio" name="role" value="admin" disabled v-model="role">
                  <label for="adminradio"> Admin</label>
                  <input class="is-checkradio is-small is-warning" id="superadminradio" type="radio" name="role" value="superadmin" disabled v-model="role">
                  <label for="superadminradio">Superadmin</label>
              </div>
              <div class="field">
                <div class="control">
                  <textarea class="textarea is-success is-rounded" placeholder="Why You Need More Previleges ?" id="message" rows="3" v-model="message" required></textarea>
                </div>
              </div>
              <div class="field">
                <div class="control">
                  <div class="b-checkbox is-success is-circular is-inline">
                    <input class="styled has-text-success" type="checkbox" id="terms" name="terms" v-model="checked">
                    <label for="terms">
                      <span class="content has-text-white">  I Accept and Read the <a class="has-text-success" href="https://raw.githubusercontent.com/tks18/gindex-v4/dark-mode-0-1/CONTRIBUTING.md" target="_blank">Community Guidelines</a></span>
                    </label>
                  </div>
                </div>
              </div>
              <div class="field">
                <div class="control">
                  <div class="b-checkbox is-success is-circular is-inline">
                    <input class="styled has-text-success" type="checkbox" id="code" name="terms" v-model="codechecked">
                    <label for="code">
                      <span class="content has-text-white">  I Accept and Read the <a class="has-text-success" href="https://github.com/tks18/gindex-v4/blob/dark-mode-0-1/CODE_OF_CONDUCT.md" target="_blank">Code of Conduct</a></span>
                    </label>
                  </div>
                </div>
              </div>
              <button :class=" loading ? 'button is-loading is-rounded is-warning is-medium' : 'button is-warning is-rounded is-medium' " type="submit" :disabled="disabled" >
                <span class="icon">
                  <i class="fas fa-user-plus"></i>
                </span>
                <span>Request Access</span>
              </button>
            </form>
          </div>
          <div class="column is-half">
            <div class="columns is-multiline is-desktop is-centered">
              <div class="column is-full">
                <div class="box has-background-warning">
                  <h2 class="title has-text-black has-text-centered has-text-weight-bold">Request More Previleges</h2>
                  <div class="content">
                    <span class="subtitle has-text-weight-bold has-text-centered">Ahh ! Why You need to Have More Access to this Website.</span>
                  </div>
                </div>
                <div :class=" superadminmodal ? 'modal is-active' : 'modal' ">
                  <div class="modal-background"></div>
                  <div class="modal-card">
                    <header class="modal-card-head">
                      <p class="modal-card-title has-text-centered">Super Admin Features</p>
                      <button class="delete" @click="superadminmodal = false" aria-label="close"></button>
                    </header>
                    <section class="modal-card-body">
                      <div class="columns is-multiline is-desktop">
                        <div class="column is-full">
                          <p class="subtitle has-text-weight-bold has-text-centered">Features</p>
                        </div>
                        <div class="column is-full">
                          <p class="subtitle has-text-weight-semibold">Permissions Related to New Users:</p>
                        </div>
                        <div class="column is-full">
                          <p class="subtitle">Following Permissions are Related to New Users</p>
                        </div>
                        <div class="column is-full">
                          <ul>
                            <li> Accept New Users</li>
                            <li> Invite New Users through Glory to Heaven Mail Service.</li>
                          </ul>
                        </div>
                        <div class="column is-full">
                          <p class="subtitle has-text-weight-semibold">Granting Permissions to Existing Users:</p>
                        </div>
                        <div class="column is-full">
                          <p class="subtitle">Following Permissions are Related to Granting of Permissions</p>
                        </div>
                        <div class="column is-full">
                          <ul>
                            <li> Promote a User to Admin</li>
                            <li> Promote a Admin to Superadmin</li>
                            <li> Invite a Admin for Superadmin Role throught Glory to Heaven mail Service</li>
                            <li> Invite a user for Admin Role throught Glory to Heaven mail Service</li>
                          </ul>
                        </div>
                        <div class="column is-full">
                          <p class="subtitle has-text-weight-semibold">Permission Related to Existing Users:</p>
                        </div>
                        <div class="column is-full">
                          <p class="subtitle">Following Permissions are Related to Handling of Existing Users</p>
                        </div>
                        <div class="column is-full">
                          <ul>
                            <li> Delete a User</li>
                            <li> Delete a Admin</li>
                            <li> Delete a Superadmin</li>
                            <li> Add a User to Spam</li>
                          </ul>
                        </div>
                      </div>
                    </section>
                  </div>
                </div>
                <div class="box has-background-danger">
                  <h2 class="title has-text-white has-text-centered has-text-weight-bold">What Previleges You Get?</h2>
                  <p class="subtitle has-text-weight-bold has-text-centered"> Click the Below Buttons to See the Features</p>
                  <div class="columns is-multiline is-mobile is-centered">
                    <div v-if="!admin && !superadmin" class="column is-two-thirds">
                      <p class="subtitle has-text-white">For Admin Features</p>
                    </div>
                    <div v-if="!admin && !superadmin" class="column is-one-third">
                      <button class="button is-white is-rounded" @click="adminmodal = true">
                        <span class="icon is-small">
                          <i class="fas fa-sticky-note"></i>
                        </span>
                        <span class="content">Click Here</span>
                      </button>
                    </div>
                    <div v-if="admin && !superadmin" class="column is-two-thirds">
                      <p class="subtitle has-text-white">For Superadmin Features</p>
                    </div>
                    <div v-if="admin && !superadmin" class="column is-one-third">
                      <button class="button is-white is-rounded" @click="superadminmodal = true">
                        <span class="icon is-small">
                          <i class="fas fa-sticky-note"></i>
                        </span>
                        <span class="content">Click Here</span>
                      </button>
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
        props : ["nextUrl"],
        data(){
            return {
                user: {},
                admin: false,
                superadmin: false,
                resultmessage: "",
                checked: "",
                codechecked: "",
                disabled: "",
                adminmodal: false,
                superadminmodal: false,
                errorMessage: false,
                successMessage: false,
                role: "",
                apiurl: "",
                loading: false,
                fullpage: true,
                message: "",
            }
        },
        methods : {
            handleSubmit(e) {
              this.loading = true;
                e.preventDefault()
                if(this.checked && this.codechecked){
                  this.$http.post(this.apiurl, {
                        name: this.user.name,
                        email: this.user.email,
                        message: this.message,
                  })
                  .then(response => {
                      if(response){
                        if(response.data.auth && response.data.registered){
                          this.successMessage = true;
                          this.errorMessage = false;
                          this.loading = false;
                          this.resultmessage = response.data.message
                        } else {
                          this.successMessage = false;
                          this.errorMessage = true;
                          this.loading = false;
                          this.resultmessage = response.data.message
                        }
                      }
                  })
                  .catch(error => {
                      console.error(error);
                  });
                } else {
                  this.successMessage = false;
                  this.errorMessage = true;
                  this.loading = false;
                  this.resultmessage = "You Need to Accept Community Guidelines."
                }
            },
            gotoPage(url){
              this.$router.push(url)
            },
        },
        beforeMount() {
          this.loading = true;
          var token = localStorage.getItem("tokendata");
          var user = localStorage.getItem("userdata");
          if (user != null && token != null){
            var userData = JSON.parse(this.$hash.AES.decrypt(user, this.$pass).toString(this.$hash.enc.Utf8));
            this.user = userData, this.loading = false;
          } else {
            this.user = null, this.loading = false;
          }
        },
        mounted(){
          this.loading = true;
          if(!this.user.admin && !this.user.superadmin){
            this.apiurl = window.apiRoutes.requestadminroute;
            this.admin = false, this.superadmin = false, this.role = 'admin', this.loading = false;
          } else if(this.user.admin && !this.user.superadmin) {
            this.apiurl = window.apiRoutes.requestsuperadminroute;
            this.admin = true, this.superadmin = false, this.role = "superadmin", this.loading = false;
          } else {
            this.loading = false;
            this.$router.push({ name: 'results', params: { id: 0, cmd: "result", success: false, data: "You are Already a Admin or SuperAdmin", redirectUrl: "/0:home/" } })
          }
        },
        watch: {
          codechecked: function() {
              if(this.role.length > 0 && this.checked && this.codechecked && this.message.length > 0){
                this.disabled = false;
              } else {
                this.disabled = true;
              }
          }
        },
    }
</script>
