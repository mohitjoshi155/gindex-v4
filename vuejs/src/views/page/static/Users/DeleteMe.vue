<template>
    <div class="content mt-4">
      <div class="loading">
        <loading :active.sync="loading" :can-cancel="false" :is-full-page="fullpage"></loading>
      </div>
      <div class="columns is-multiline has-text-centered is-desktop is-centered is-vcentered">
        <div class="column is-full">
          <h2 class="title has-text-weight-bold has-text-white">Delete Your Account</h2>
          <p class="is-small has-text-danger-dark has-text-weight-bold">Be Cautious !!</p>
        </div>
        <div class="column is-half">
          <article :class=" errorMessage ? 'message is-danger' : 'message is-hidden is-danger'">
            <div class="message-header">
              <p>Error Proccessing</p>
              <button class="delete" @click="errorMessage = false" aria-label="delete"></button>
            </div>
            <div class="message-body">
              {{ resultmessage }}
            </div>
          </article>
          <form @submit.prevent="handleSubmit">
            <div class="field">
              <p class="control has-icons-left">
                <input class="input is-rounded" id="password" type="password" placeholder="Your Password" v-model="password" required>
                <span class="icon is-small is-left">
                  <i class="fas fa-lock"></i>
                </span>
              </p>
            </div>
            <button :class=" loading ? 'button is-loading is-rounded is-warning' : 'button is-warning is-rounded' " type="submit" :disabled="disabled">
              <span class="icon">
                <i class="fas fa-user-minus"></i>
              </span>
              <span>Delete</span>
            </button>
          </form>
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
                password : "",
                resultmessage: "",
                disabled: true,
                errorMessage: false,
                loading: false,
                fullpage: true,
            }
        },
        methods : {
            handleSubmit(e) {
              this.loading = true;
                e.preventDefault()
                if (this.password && this.password.length > 0)
                {
                    let url = window.apiRoutes.deleteMe
                    this.$http.post(url, {
                          email: this.user.email,
                          pass: this.password,
                    })
                    .then(response => {
                        if(response){
                          if(response.data.auth && response.data.registered && response.data.deleted){
                            this.resultmessage = response.data.message
                            localStorage.removeItem('userdata');
                            localStorage.removeItem('tokendata');
                            setTimeout(() => {
                              this.loading = false;
                              this.$router.push({ name: 'results', params: { id: 0, cmd: "result", success: true, data: "You Account is Being Deleted. Please Wait", redirectUrl: "/0:home/" } })
                            }, 1500)
                          } else {
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
                    this.errorMessage = true;
                    this.loading = false;
                    this.resultmessage = "Fill in Your Password"
                    this.password = "";
                }
            },
        },
        beforeMount() {
          var user = localStorage.getItem("userdata");
          var token = localStorage.getItem("tokendata");
          if(user && token){
            var userData = JSON.parse(this.$hash.AES.decrypt(user, this.$pass).toString(this.$hash.enc.Utf8));
            this.user = userData, this.loading = false;
            console.log(this.user);
          } else {
            this.user = null, this.loading = false;
          }
        },
        watch: {
          password: function() {
            if(this.password.length > 0){
              this.disabled = false;
            } else {
              this.disabled = true;
            }
          }
        },
    }
</script>
