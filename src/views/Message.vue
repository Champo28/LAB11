<template>
<div class="background">
	<div class="container message_container">
		<h1 class="text">{{ message }}</h1>
	</div>
</div>
</template>

<script>

import { useUserStore } from '@/store/user'

export default {
	setup() {
		const userStore = useUserStore()
		return { userStore }
  	},	

    data() {
		return {
            message: '',
		}
	},   
    mounted() {
		this.setMessage();
	},

	methods: {
		setMessage() {
			if (this.$route.params.id == 1) {
				this.message = "Welcome! You can now login"
			} 
			if (this.$route.params.id == 2) {
				this.message = "Welcome back " + this.userStore.getUser.name + "!"
			}
			if (this.$route.params.id == 3) {
				this.message = "Success: CheckOut Done"
			}
			if (this.$route.params.id == 4) {
				this.message = "See you back soon!"
			}														
			
			var timeleft = 2;
			var downloadTimer = setInterval(() => {
				if(timeleft <= 0){
					clearInterval(downloadTimer)
					this.$router.push('/')
				}
			timeleft -= 1
			}, 1000)
		},
	}

}
</script>

<style scoped>

.background{
	height: 100vh;
    background-image: url('../assets/images/test.gif');
    background-attachment: fixed;
    background-repeat: no-repeat;
    background-size: 100%;
    background-position: center;
}

.message_container {
    width: 90%;
    height: 500px;
    display: flex;
    text-align: center;
    justify-content: center;
    align-items: center;
    position: relative;
    margin: auto;
}

.text {
    letter-spacing: 10px;
    font-family: monospace;
    border-right: 5px solid;
    width: 100%;
    white-space: nowrap;
    overflow: hidden;
    color: white;
    text-shadow: 1px 3px 2px rgba(153, 153, 153, 1);
    animation: typing 3s, cursor 0.4s step-end infinite alternate;
}

@keyframes cursor {
    50% {border-color: transparent;}
}

@keyframes typing {
    from {
        width: 0;
        margin: 0 auto;
    }
}

</style>



