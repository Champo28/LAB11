<template>
<div>

	<Header />
	<div class="grid-container">
		<div class="left-panel">
			<div class="container">
				<h2>Categories</h2><hr>
				<h4 class="my-2" @click="showAll" style="cursor: pointer;"><img src="../assets/images/all.png" width="25" height="25"/>  <u>All</u></h4>
				<div v-for="categorie in categories" :key="categorie.id">
					<div class="my-2">
						<h4 @click="showCategorie(categorie.name)" style="cursor: pointer;"><img :src="getImagePath(categorie.image)" alt="Category Image" referrerpolicy="no-referrer" width="25" height="25"/>  <u>{{ categorie.name }}</u></h4>
					</div>
				</div>
			</div>
		</div>
		<div class="right-panel">
			<h2 class="center">Products</h2><hr>
			<div class="image-grid">
				<div v-for="product in products" :key="product.id" :categorie-data=product.cat_id>
					<div><img :src=getImagePath(product.image) height="340" width="340"/>
						<div class="center">
							<h6 class="my-3" style="font-style: italic; font-weight: bold;">{{ product.name }}</h6>
							<button class="btn button-62 shadowEffect" style="width: 200px;" @click="addProductToBasket(product)"><i class="bi bi-cart"></i>  {{  product.price }}$</button>
						</div><br>
					</div>
				</div>
			</div>
		</div>
	</div>
	

	<Footer />

</div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

import { useBasketStore } from '@/store/basket'
import { useProductsStore } from '@/store/products'
import { useUserStore } from '@/store/user'
import { useCategoriesStore } from '@/store/categories'

export default {
	setup() {
		const basketStore = useBasketStore()
		const productsStore = useProductsStore()
		const userStore = 	useUserStore()
		const categoriesStore = useCategoriesStore()	
		return { basketStore, productsStore, userStore, categoriesStore }
  	},
    components: {
		Footer,
        Header
	},
	data() {
		return {
			categories: [],
			products: [],
		}
	},
	mounted() {
		this.categoriesStore.getCategoriesDB().then(result => {
			if(result){
				this.categories = this.categoriesStore.getCategories;
			}
		});
		
		this.productsStore.getProductsDB().then(result => {
			if(result){
				this.products = this.productsStore.getProducts;
			}
		})
	},
	methods: {
		getImagePath(imageName){
			return new URL(`../assets/images/${imageName}`, import.meta.url).href;
		},

		showAll(){
			this.categories.forEach(categorie => {
				let products = document.querySelectorAll('[categorie-data="'+categorie.name+'"]');
				products.forEach(product => {
					product.style.display = '';
				});
			});
		},

		showCategorie(categorie_name){
			this.categories.forEach(categorie => {
				if(categorie_name !== categorie.name){
					let products = document.querySelectorAll('[categorie-data="'+categorie.name+'"]');
					products.forEach(product => {
						product.style.display = 'none';
					});
				}
				else{
					let products = document.querySelectorAll('[categorie-data="'+categorie_name+'"]');
					products.forEach(product => {
						product.style.display = '';
					});
				};
			});
		},

		addProductToBasket(product){
			this.basketStore.incrementProduct(product.id);
		}
	},

    computed: {

    },
}



</script>

<style scoped>

.grid-container{
	display: grid;
  	grid-template-columns: 20% 80%;
  	min-height: 100vh;
}

.image-grid{
	display: flex;
  	flex-wrap: wrap;
  	justify-content: space-between;
	padding: 30px;
}
.left-panel {
	padding: 20px;
}

.right-panel {
	padding: 20px;
}

.center{
	text-align: center;
	vertical-align: middle;
}

img {
  transition: transform 0.3s ease-in-out; 
}

img:hover {
  transform: scale(1.05);
}

h2{
	font-family: 'Consolas', 'monaco', 'monospace';
	font-style: italic;
	font-weight: bolder;
}

.shadowEffect {
    border-radius: 15px;
    box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.5);
}

/* CSS */
.button-62 {
  background: linear-gradient(to bottom right, #EF4765, #FF9A5A);
  border: 0;
  border-radius: 12px;
  color: #FFFFFF;
  cursor: pointer;
  display: inline-block;
  font-family: -apple-system,system-ui,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
  font-size: 16px;
  font-weight: 500;
  line-height: 2.5;
  outline: transparent;
  padding: 0 1rem;
  text-align: center;
  text-decoration: none;
  transition: box-shadow .2s ease-in-out;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
}

.button-62:not([disabled]):focus {
  box-shadow: 0 0 .25rem rgba(0, 0, 0, 0.5), -.125rem -.125rem 1rem rgba(239, 71, 101, 0.5), .125rem .125rem 1rem rgba(255, 154, 90, 0.5);
}

.button-62:not([disabled]):hover {
  box-shadow: 0 0 .25rem rgba(0, 0, 0, 0.5), -.125rem -.125rem 1rem rgba(239, 71, 101, 0.5), .125rem .125rem 1rem rgba(255, 154, 90, 0.5);
}

</style>