<template>
<div>
    <Header />
    <div class="center py-3">
        <h2 v-if="orders === undefined">No orders available</h2>
        <h2 v-else>My Orders</h2>
    </div>
    <div class="container">
        <table class="table table-bordered">
            <thead class="center">
                <tr>
                    <th scope="column">ID</th>
                    <th scope="column">Date</th>
                    <th scope="column">Total</th>
                    <th scope="column">Status</th>
                    <th scope="column">Invoice</th>
                </tr>
            </thead>
            <tbody v-for="order in orders" :key="order.id" class="center">
                <tr>
                    <th scope="row">{{ order.id }}</th>
                    <td>{{ order.created_at }}</td>
                    <td>{{ order.total }}$</td>
                    <td>{{ order.status_id }}</td>
                    <td><router-link :to="'/invoice/'+order.id">Show</router-link></td>
                </tr>
            </tbody>
        </table>
    </div>

    <Footer />

</div>
</template>

<script>

import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

import { useOrdersStore } from '@/store/orders'

export default {
    setup() {
        const ordersStore = useOrdersStore()
        return { ordersStore }
    },

    components: {
        Footer,
        Header
    },

    data() {
        return {
            isHidden: false,
            id: 0,
            orders:[
              //{
              //"id":"11",
              //"customer_id":"20",
              //"created_at":"2021-12-03 18:20:31",
              //"status_id":"1",
              //"total":"190",
              //"order_items":[{"product_id":"2","name":"Salmon Roll","price":"18","quantity":"4"},{"product_id":"3","quantity":"3"},{"product_id":"4","quantity":"2"}]
              //}
            ]
        }
    },

    mounted() {
        this.ordersStore.getMyOrdersDB().then(() => {
            this.orders = this.ordersStore.getOrders;
        });
    },

    methods: {
    
    },

    computed: {

    },
}
</script>

<style scoped>

.center{
	text-align: center;
    vertical-align: middle;
}

</style>