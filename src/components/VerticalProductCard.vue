<script setup lang="ts">
import { useCartStore } from '@/stores/cart'
import { RouterLink } from 'vue-router'

defineProps<{
  product: {
    id: number
    title: string
    price: number
    description: string
    category: string
    image: string
    rating: {
      rate: number
      count: number
    }
  }
}>()

const cartStore = useCartStore()
</script>

<template>
  <div class="col-lg-3 col-md-4 col-sm-6 mt-40">
    <!-- single-product-wrap start -->
    <div class="single-product-wrap">
      <div class="product-image">
        <RouterLink :to="{ name: 'product', params: { id: product.id } }">
          <div :style="{ padding: '0 20px' }">
            <img :src="product.image" :alt="product.title" />
          </div>
        </RouterLink>
        <span class="sticker">New</span>
      </div>
      <div class="product_desc">
        <div class="product_desc_info">
          <div class="product-review">
            <h5 class="manufacturer">
              <RouterLink :to="{ name: 'product', params: { id: product.id } }">{{
                product.category
              }}</RouterLink>
            </h5>
            <div class="rating-box">
              <ul class="rating">
                <li><i class="fa fa-star-o"></i></li>
                <li><i class="fa fa-star-o"></i></li>
                <li><i class="fa fa-star-o"></i></li>
                <li class="no-star"><i class="fa fa-star-o"></i></li>
                <li class="no-star"><i class="fa fa-star-o"></i></li>
              </ul>
            </div>
          </div>
          <h4>
            <RouterLink
              class="product_name"
              :to="{ name: 'product', params: { id: product.id } }"
              >{{ product.title }}</RouterLink
            >
          </h4>
          <div class="price-box">
            <span class="new-price">${{ product.price }}</span>
          </div>
        </div>
        <div class="add-actions">
          <ul class="add-actions-link">
            <li class="add-cart active">
              <a href="#" @click="() => cartStore.addToCart(product, 1)">Add to cart</a>
            </li>
            <li>
              <a
                href="#"
                title="quick view"
                class="quick-view-btn"
                data-toggle="modal"
                data-target="#exampleModalCenter"
                ><i class="fa fa-eye"></i
              ></a>
            </li>
            <li>
              <a class="links-details" href="#"><i class="fa fa-heart-o"></i></a>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <!-- single-product-wrap end -->
  </div>
</template>
