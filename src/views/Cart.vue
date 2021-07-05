<template>
    <div>
        <Header/>
            <!-- Breadcrumb Section Begin -->
            <div class="breacrumb-section">
                <div class="container">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="breadcrumb-text product-more text-left">
                                 <router-link to="/"> 
                                        <i class="fa fa-home"></i> Home
                                </router-link>
                                <span>Shopping Cart</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!-- Breadcrumb Section Begin -->
            <ShoppingCart/>
    </div>
</template>

<script>
import Header from '@/components/Header.vue'
import ShoppingCart from '@/components/ShoppingCart.vue'

export default {
    name: 'Cart',
    components: {
        Header, 
        ShoppingCart
    }
}
</script>

<style scoped>

</style>