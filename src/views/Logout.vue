<template>
	<div>

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
			success: false,
			user: {},
		}
	},


	methods: {
		
	},
   
	mounted() {
		this.userStore.logoutUserDB().then(result => {
            if(result){
                this.$router.push({path: '/message/4'})
            }
		});
	},
	
	computed: {

	}

}
</script>

<style scoped>

</style>