<template>
<div class="page-category">
    <div class="columns is-multiline">
          <div class="column is-12">
            <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
          </div>
    </div>
    <ProductBoxVue
                v-for="product in category.products"
                v-bind:key="product.id"
                v-bind:product="product"
    />

</div>

</template>

<script>
import axios from 'axios'
import ProductBoxVue from '@/components/ProductBox.vue'
export default{
    name: 'Category',
    data(){
        return{
            category:{
                products:[]
            }
        }

    },
    components:{
        ProductBoxVue

    },
    mounted() {
        this.getCategory()

    },
    watch:{
        $route(to, from){
            if(to.name === 'category'){
                this.getCategory()
            }
        }
    },
    methods:{
        async getCategory(){
            this.$store.commit('setIsLoading', true)
            const categorySlug = this.$route.params.category_slug
            await axios
            .get(`/api/v1/products/${categorySlug}/`)
            .then(response=>{
                this.category = response.data
                document.title = this.category.name + ' | Jackets'
            })
            .catch(error=>{
                console.log(error)

                toast({
                   message: 'Something went wrong, try again',
                   type: 'is-danger',
                   dismissible : true,
                   pauseOnHover: true,
                   duration: 2000,
                   position:'bottom-right',
               })
            })
            this.$store.commit('setIsLoading', false)

            
        },

    }

}

</script>