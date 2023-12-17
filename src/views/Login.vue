<template>
<div class="background">
  
	<Header />

	<div class="container my-3 shadowEffect login_container">
		<div class="row">
			<div class="center my-3">			
				<ul>
					<li><img src="../assets/images/login_star.gif" width="50" height="50"></li>
					<li><h2 style="display: inline; color: white; font-style: italic;">Login</h2></li>
					<li><img src="../assets/images/login_star.gif" width="50" height="50"></li>
				</ul>
			</div>
			<form @submit.prevent="checkForm" method="post">
				<div class="mx-auto col-sm-12 col-md-8 col-lg-8 col-xl-8 my-3">
					<label for="email" class="form-label" style="font-style: italic; color: #E1D9D1">Email</label>
					<input type="text" class="form-control shadowEffect" id="email" name="email" v-model="user.email">
				</div>
				<div class="mx-auto col-sm-12 col-md-8 col-lg-8 col-xl-8 my-3">
					<label for="password" class="form-label" style="font-style: italic; color: #E1D9D1">Password</label>
					<input type="password" class="form-control shadowEffect" id="password" name="password" v-model="user.password">
				</div>
				<div class="mx-auto col-sm-12 col-md-8 col-lg-8 col-xl-8 my-3">
					<input type="checkbox" id="autologin" name="autologin" value="1">
					<label for="autologin" style="font-style: italic; color: white;">Remember me</label>
					<router-link to='/register' style="font-style:italic; margin-left: 35%;">Forgot Password?</router-link>
				</div>
				<div class="center py-3">
					<button class="button-85 shadowEffect" type="submit" id="signUp">Submit</button>
				</div>

				<div class="mx-auto col-sm-12 col-md-8 col-lg-8 col-xl-8 py-3 center">
                    <p style="font-style: italic; color: white;">Don't have an account? 
                    <router-link to='/register'>Sign Up</router-link></p>
                </div> 
			</form>
			
		</div>
	</div>

	<Footer />

</div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

import { useUserStore } from '@/store/user'

export default {

	setup() {
		const userStore = useUserStore()
		return { userStore }
  	},
		
	components: {
		Footer,
        Header
	},   
	data() {
      return {	
        user: {
			email: '',
			password: '',
        },
		submitting: false,
		error: false,
      }
    },
	
	methods: {
		checkForm(){
			if(this.user.email === '' || this.user.password === ''){
				this.error = true;
				return;
			}

			this.userStore.loginUserDB(this.user).then(result => {
				if(result){
					this.$router.push({path: '/message/2'})
				}
			});
		}
	},
	
	computed: {

	},
	directives: {

	},
	created: function () {

	}
}
</script>

<style scoped>
.login_container{
	width: 600px;
	height: auto;
	border: 2px solid #ffffff;
	border-radius: 30px;
}

.center{
	text-align: center;
    vertical-align: middle;
}

li { 
    display: inline;
	padding: 30px; 
}

.shadowEffect{
    box-shadow: 2px 2px 6px lime;
}

.background{
	min-height: 100vh;
	flex: 1;
	background-color: #121212;
}

.form-control{
	background-color: whitesmoke; 
}

/* CSS created by CSS Scan*/
.button-85 {
  padding: 0.6em 2em;
  border: none;
  outline: none;
  color: rgb(255, 255, 255);
  background: #111;
  cursor: pointer;
  position: relative;
  z-index: 0;
  border-radius: 10px;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-85:before {
  content: "";
  background: linear-gradient(
    45deg,
    #ff0000,
    #ff7300,
    #fffb00,
    #48ff00,
    #00ffd5,
    #002bff,
    #7a00ff,
    #ff00c8,
    #ff0000
  );
  position: absolute;
  top: -2px;
  left: -2px;
  background-size: 400%;
  z-index: -1;
  filter: blur(5px);
  -webkit-filter: blur(5px);
  width: calc(100% + 4px);
  height: calc(100% + 4px);
  animation: glowing-button-85 20s linear infinite;
  transition: opacity 0.3s ease-in-out;
  border-radius: 10px;
}

@keyframes glowing-button-85 {
  0% {
    background-position: 0 0;
  }
  50% {
    background-position: 400% 0;
  }
  100% {
    background-position: 0 0;
  }
}

.button-85:after {
  z-index: -1;
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background: #222;
  left: 0;
  top: 0;
  border-radius: 10px;
}

</style>