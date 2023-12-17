<template>
<div>

    <div class="container">
        <header>
            <p style="font-weight: bold;"><router-link to='/menu'><img src="../assets/images/godjo.png" alt="Logo"></router-link>Limitless Gaming Store</p> 
            <h1>Invoice</h1>      
        </header><hr>
        <div class="container header-container">
            <div class="left">
                <h3>To: </h3>
                <h6>{{ user.name }}</h6>
                <h6>Adress: XXXXXXXXXX</h6>
                <h6>Phone Number: XXXXXXXXX</h6>
                <h6>Payment Method: Credit Card</h6>
            </div>
            <div class="right">
                <h5>Invoice #{{ order.id }}</h5>
                <h5>Date: {{ order.created_at }}</h5>
            </div>           
        </div><hr>
        <div class="row">
            <table class="table table-bordered table-hover">
                <thead class="center">
                    <tr>
                        <th scope="column">ID</th>
                        <th scope="column">Description</th>
                        <th scope="column">Price</th>
                        <th scope="column">Quantity</th>
                        <th scope="column">Sub Total</th>
                    </tr>
                </thead>
                <tbody v-for="item,index in order.items" :key="item.id" class="center">
                    <tr>
                        <th scope="row">{{ item.product_id }}</th>
                        <td>{{ products[index].description }}</td>
                        <td>{{ item.price }}$</td>
                        <td>{{ item.quantity }}</td>
                        <td>{{ item.price * item.quantity }}$</td>
                    </tr>
                </tbody>
            </table>
            <div class="right">
                <h6>Sub Total:&ensp;&ensp;&ensp;{{ order.total }}$</h6>
                <h6>Tax: &ensp;&ensp;&ensp;N/A</h6>
                <h6>Total:&ensp;&ensp;&ensp;{{ order.total }}$</h6>
            </div>
        </div>
    </div><br>
    <div class="container">
        <div>
            <h5>Thanks for buying!</h5>
            <h6 style="font-style: italic;">Contact us: limitlessShop@shop.com</h6>
        </div>       
    </div>

</div>
</template>

<script>

import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

import { useOrdersStore } from '@/store/orders'
import { useProductsStore } from '@/store/products'
import { useUserStore } from '@/store/user'

export default {
    setup() {
        const productsStore = useProductsStore()
        const ordersStore = useOrdersStore()
        const userStore = useUserStore()
        return { ordersStore, productsStore, userStore }
    },

    components: {
        Footer,
        Header
    },

    data() {
        return {
            order: {
                id: '',
                user_id: '',
                created_at: '',
                status_id: '',
                total: '',
                items: []
            },
            products: [],
            user: '',
        }
    },

    mounted() {
        const id = this.$route.params.id;
        this.order = this.ordersStore.getOrder(id);
        this.order.items.forEach(item => {
            let p = this.productsStore.getProduct(item.product_id);          
            this.products.push(p);
        });
        this.user = this.userStore.getUser;
    },

    methods: {
    },

    computed: {

    },
}
</script>

<style scoped>

header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px; 
    color: black;
}

header img {
    width: 80px; 
    height: 80px; 
    margin-right: 10px; 
}

header h1 {
    margin: 0;
}
.center{
    text-align: center;
    vertical-align: middle;
}

.right{
    text-align: right;
}

.left{
    text-align: left;
}
.header-container{
    display: flex;
    justify-content: space-between; 
    align-items: center; 
}

</style>