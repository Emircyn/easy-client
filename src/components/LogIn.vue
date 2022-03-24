<script>
import axios from "axios";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import { reactive, computed } from "vue";
export default {
  name: 'LogIn',
   setup() {
    const state = reactive({
      username: null,
      pass: null,
      
    });
    const rules = computed(() => {
      return {
        username: { required },
        pass: { required },
      };
    });

    const v$ = useVuelidate(rules, state);
    return {
      state,
      v$,
    };
  },
  data(){
      return{
          info: null,
          loading:false

      }
  },
  created(){
      console.log(import.meta.env.VITE_APP_API_USERS+"/login?username="+
              this.state.username +
              "&pass=" +
              this.state.pass);
  },
  methods: {
    login(event) {
      event.preventDefault();
      this.v$.$validate();
      if (!this.v$.$error) {
          this.loading=true;
          setTimeout(() => {
               axios
          .get(import.meta.env.VITE_APP_API_USERS+"/login?username="+
              this.state.username +
              "&pass=" +
              this.state.pass
          )
          .then((response) => {
            this.info = response.data.length;
            console.log(this.info);
            if (this.info == 1) {
             console.log("giriş başarılı");
              this.loading=false;
            } else {
              console.log("giriş başarısız");
              this.loading=false;
            }
          });
          }, 750);
       
      }
    },
  },
  
}
</script>
<template>
  <nav class="login-grid">
    <div class="login-header">
      <h2>welcome to</h2>
      <img class="logo" src="../assets/icon/Logo.svg" draggable="false" />
    </div>
    <div class="card login-card">
      <div class="switcher">
        <Router-Link to="/login"> login </Router-Link>
        <Router-Link to="/signup"> sign up </Router-Link>
      </div>
      <form>
        <div class="input-item">
          <div>Username</div>
          <input
           v-model="state.username"
            type="text"
            placeholder="Enter your username."
            required
          />
          <span v-if="v$.username.$error">
            {{ v$.username.$errors[0].$message }}
          </span>
        </div>
        <div class="input-item">
          <div>Password</div>
          <input
             v-model="state.pass"
            type="password"
            placeholder="Enter your password."
            required
          />
         <span v-if="v$.pass.$error">
            {{ v$.pass.$errors[0].$message }}
          </span>
        </div>
        <div class="input-item">
          <button @click="login" type="submit"><span v-if="!loading">SIGN IN</span>
          <svg v-if="loading" version="1.1" id="L9" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
  viewBox="0 0 100 100" enable-background="new 0 0 0 0" xml:space="preserve">
    <path fill="#fff" d="M73,50c0-12.7-10.3-23-23-23S27,37.3,27,50 M30.9,50c0-10.5,8.5-19.1,19.1-19.1S69.1,39.5,69.1,50">
      <animateTransform 
         attributeName="transform" 
         attributeType="XML" 
         type="rotate"
         dur="1s" 
         from="0 50 50"
         to="360 50 50" 
         repeatCount="indefinite" />
  </path>
</svg>
          </button>
        </div> 
      </form>
    </div>
    <div class="login-bottom">
      <a href="">what is </a><a href="" style="color: var(--red)">easy?</a>
    </div>
  </nav>
</template>
