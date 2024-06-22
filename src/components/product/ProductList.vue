<template>
    <h5>Listado de Productos</h5>
    <div class="product-list">
        <div class="product-grid">
            <div class="product-item" v-for="product in products" :key="product.id">
                <ProductItem :product="product" />
            </div>
        </div>
    </div>
</template>

<style>
.product-grid{
    display: grid;
    grid-template-columns: repeat(3,1fr) ;
    grid-gap: 20xp;
}

</style>

<script>
import ProductItem from 'src/components/product/ProductItem.vue'

export default{
    name:"ProductList",
    components: {ProductItem},
    data(){
        return {
            products: []
        }
    },
    mounted(){
        this.loadProducts()
    },
    methods:{
        loadProducts(){
            var URL = "/product"
            var token = JSON.parse(localStorage.getItem("userData")).result.token
            console.log("TOKEN es : " + token)
            this.$api.get(URL,{
                headers: {
                    Authorization: 'Bearer ' + token
                }
            })
            .then(response=>{
                this.products = response.data
                console.log(JSON.stringify(response))
            }).catch(error=>{
                console.log("Ocurrio un error: " + error)
                this.$router.push("/")
            })
        }
    }
}


</script>