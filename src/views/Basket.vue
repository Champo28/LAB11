<template>
<div>
  
	<Header /><br>
	<div v-if="basket.length > 0" class="container">
		<h3 class="center py-3">My Cart</h3>
		<table class="table">
			<thead class="center">
				<tr>
					<th scope="col">ID</th>
					<th scope="col">Name</th>
					<th scope="col">Price</th>
					<th scope="col">	</th>
					<th scope="col">Quantity</th>
					<th scope="col">	</th>
					<th scope="col">Sub total</th>
					<th scope="col">	</th>
				</tr>
			</thead>
			<tbody v-for="basketProduct,index in basket" :key="basketProduct.id" class="center">
				<tr :id=basketProduct.id>
					<th scope="row">{{ basketProduct.id }}</th>
					<td>{{ products[index].name }}</td>
					<td>{{ products[index].price }}$</td>
					<td><button @click="increment(basketProduct)" class="btn btn-primary"><i class="bi bi-plus-lg"></i></button></td>
					<td>{{ basketProduct.quantity }}</td>
					<td><button @click="decrement(basketProduct)" class="btn btn-primary"><i class="bi bi-dash-lg"></i></button></td>
					<td> {{  basketProduct.quantity * products[index].price  }}$</td>
					<td @click="remove(basketProduct)" style="cursor: pointer;"><u>Remove</u></td>
				</tr>
			</tbody>
		</table>
		<div class="center">
			<h5 class="right py-3" v-if="total !== 0">Total:&ensp;&ensp;&ensp;{{ total }}$</h5>
			<div class="my-5">
				<router-link to="/login" v-if="user.session_id === undefined"><button class="btn button-85">Login First to Checkout</button></router-link>
				<button v-else @click="checkOut" class="btn button-85">Checkout</button>
				<router-link to='/menu'><button class="btn button-85 mx-5">Continue Shopping</button></router-link>
			</div>
		</div>
	</div>
	<div v-else class="center container cart-container py-3">
		<div class="py-3">
			<img src="../assets/images/cart_is_empty.png" class="py-3" width="250" height="250">
			<h3 class="py-3">Your Cart is Empty</h3>
			<h6>Must add items on the cart before you proceed to check out</h6>
		</div>
		<router-link to='/menu'><button class="btn button-85 my-5">Return to shop</button></router-link>
	</div>

</div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

import { useBasketStore } from '@/store/basket'
import { useOrdersStore } from '@/store/orders'
import { useProductsStore } from '@/store/products'
import { useUserStore } from '@/store/user'

export default {
	setup() {
		const basketStore = useBasketStore()
		const ordersStore = useOrdersStore()
		const productsStore = useProductsStore()
		const userStore = useUserStore()	
		return { basketStore, ordersStore, productsStore, userStore }
  	},
    components: {
		Footer,
        Header
	},
	data() {
		return {
			order: {
				status_id: 0,
    			totalAmount: '',
    			items: [],
			},
			user: {
				id: '', 
				name: '', 
				email: '', 
				session_id: ''
			},
			basket: [],
			products: [],
			total: 0,
		}
	},

	mounted() {
		this.basket = this.basketStore.getProducts;
		this.basket.forEach(product => {
			let p = this.productsStore.getProduct(product.id);
			this.products.push(p);
			this.total += p.price * product.quantity;
		});
		this.user = this.userStore.getUser;
	},

	methods: {
		increment(product){
			this.basketStore.incrementProduct(product.id);
			this.total += Number(this.productsStore.getProduct(product.id).price);
		},
		
		decrement(product){
			if(product.quantity === 1){
				return;
				
			}else{
				this.total -= Number(this.productsStore.getProduct(product.id).price);
				this.basketStore.decrementProduct(product.id);
				console.log(this.products);
				console.log(this.basket);
				return;
			}
		},

		remove(product){
			let p = document.getElementById(`${product.id}`);
			this.total -= Number(this.productsStore.getProduct(product.id).price);
			this.basketStore.decrementProduct(product.id);
			this.products = this.products.filter(p => {
				return p.id != product.id;
			});
			console.log(this.basket);
			console.log(this.products);
			p.remove();
		},

		checkOut(){
			this.order.status_id = 1;
			this.order.totalAmount = this.total;
			this.basket.forEach((product, index) => {
				console.log(product);
				this.order.items.push(								
					{id: product.id, name: this.products[index].name, price: this.products[index].price, quantity: product.quantity}
				);			
			});
			this.ordersStore.addOrderDB(this.order).then(result => {
				if(result){
					this.basketStore.clearBasket();
					this.$router.push({path: '/message/3'});
				}
			});
		}
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

.right{
    text-align: right;
}

.cart-container{
	text-align: center;
	width: 450px;
	height: auto;
	border-radius: 20px;
	box-shadow: 0px 0px 10px 5px rgba(0, 0, 0, 0.3);
	padding: 30px;
}

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