<template>
    <div>
		<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <div class="container-fluid">
                <router-link to='/' class="navbar-brand"><img src="../assets/images/godjo.png" width="40" height="40"></router-link>
                <ul class="navbar-nav">                  
                    <li class="nav-item">
                        <router-link to='/' class="nav-link">Home <i class="bi bi-house"></i></router-link>
                    </li>
                    <li class="nav-item">
                        <router-link to='/menu' class="nav-link">Menu <i class="bi bi-menu-app"></i></router-link>
                    </li>
                </ul>
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <router-link :to=link_1 class="nav-link">{{ menu_1 }}&ensp;<i class="bi bi-infinity"></i></router-link>
					</li>
                    <li v-if="user.session_id !== undefined" class="nav-item">
                        <router-link to='/myorders' class="nav-link">My Orders&ensp;</router-link>
					</li>
                    <li class="nav-item">
                        <router-link :to=link_2 class="nav-link">{{ menu_2 }}&ensp;<i class="bi bi-yin-yang"></i></router-link>
                    </li>
                    <li class="nav-item">
                        <router-link :to=link_3 class="nav-link">Cart&ensp;<i class="bi bi-cart"></i></router-link>
                    </li>
                </ul>
            </div>
        </nav>
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
			user: {
				id: '', 
				name: '', 
				email: '', 
				session_id: '',
			},

            menu_1: 'Login',
            menu_2: 'Register',

            link_1: '/login',
            link_2: '/register',
            link_3: '/basket',
		}
	},
	mounted() {
        this.user = this.userStore.getUser;
        this.setLinksAndMenus(this.user);
	},
    methods: {
        setLinksAndMenus(user){
            console.log(user);
            if(user.session_id !== undefined){
                this.menu_1 = `Welcome ${this.user.name}`
                this.menu_2 = 'LogOut';

                this.link_1 = '';
                this.link_2 = '/logout';
                this.link_3 = '/basket';
            }
        }
	},
	computed: {

	}
}
</script> 
