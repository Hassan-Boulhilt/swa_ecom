<template>
  <div class="cart-page">   
            <h1 class="title">Cart</h1>
            
            <div class="columns is-multiline">
                <div class="column is-12 box">
                    <table class="table is-fullwidth" v-if="cartTotalLength">
                        <thead>
                            <tr>
                                <th>Product</th>
                                <th>Price</th>
                                <th>Quantity</th>
                                <th>Total</th>
                                <th></th>
                            </tr>
                        </thead>
                        <tbody>
                            <CartItem 
                            v-for="item in cart.items"
                            v-bind:key="item.id"
                            v-bind:initialItem="item" 
                            v-on:removeFromCart="removeFromCart"                            
                            />
                        </tbody>
                    </table>
                    <p class="has-text-centered" v-else>
                        Your cart is empty </p>
                </div>
                <div class="column is-12 box">
                  <h2 class="subtitle">Summary</h2>
                  <strong> $ {{ cartTotalPrice.toFixed(2)}}</strong>, {{cartTotalLength}} items
                  <hr>
                  <router-link to="/cart/checkout" class="button is-dark "> Checkout </router-link>

                </div>
            </div>

           

  </div>

</template>
<script>
import axios from 'axios'
import CartItem from '@/components/CartItem.vue'
  export default{
    name:"cart",
    components:{
        CartItem
    },
    data(){
      return {
        cart:{
          items:[],
          
        },
        
     }
    },
    mounted(){
      this.cart = this.$store.state.cart

    },
    methods:{
      removeFromCart(item){
        this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
      }
    },
    
    computed:{
      cartTotalLength(){
        return this.cart.items.reduce((acc, curVal) => {
          return acc += curVal.quantity

        },0)     
      },
      cartTotalPrice(){
        return this.cart.items.reduce((acc, curVal) => {
          return acc += curVal.product.price * curVal.quantity
        },0)
      }

    
    }
  }

</script>