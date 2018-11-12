<template>
    <div class="cart-wrapper">
        <h2>{{title}}</h2>
        <p v-if="!cart.length">No item in cart.</p>
        <div class="cart">
          <div class="item" v-for="(item, index) in cart">
            <div class="image">
              <a href="#" :href="item.url">
                <img  :src="item.image"/>
              </a>
            </div>
            <div class="info">
              <h4>{{item.name}}</h4>
              <p class="seller">{{item.seller}}</p>
              <p class="status available" v-if="item.isAvailable">In Stock</p>
              <p class="shipping" v-if="item.isEligible">Eligible for FREE Shipping & FREE Returns</p>
              <a href="#" @click="deleteItem(index)">Delete</a>
              <a href="#" class="secondary" @click="changecart(index)" v-if="isShoppingCart">Save for later</a>
              <a href="#" class="secondary" @click="changecart(index)" v-if="isMoveToCart">Move to cart</a>
            </div>
            <div class="priceitem">
	        Item Price: <span class="price">{{item.price}} $</span>
	        </div>            
          </div>
         </div>
		<div class="subtotal" v-if="cart.length">
        Subtotal ({{cart.length}} {{item}}): <span class="price">{{cartTotal}} $</span>
        </div>
      </div>  
</template>
  
<script>
export default{
    props:{
		cart:{type:Array, required:true},
		title:{type:String, required:true},
		type:{type:String, required:true}
	},
	computed:{
		cartTotal(){
			let total=0;
			this.cart.forEach((item) => {
				total+= parseFloat(item.price, 10)
			})
			return total.toFixed(2);
		},
		item(){
			const item='item';
			if(this.cart.length>1){
				const item ='items'
				return item;
			}
            return item
		},
		isShoppingCart(){
			if(this.type==='shopingcart'){
				return true
			}
		},
		isMoveToCart(){
			if(this.type==='savedcart'){
				return true
			}
		}
	},
	methods:{
		deleteItem(index){
			return this.cart.splice(index, 1)
		},
		changecart(index){
			const moveitem=this.cart.splice(index, 1);
			this.$emit('changecartitem', moveitem[0], this.type)
		}
	}    
}
</script>
