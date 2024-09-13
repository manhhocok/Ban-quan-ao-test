<script setup lang="ts">
import { useCartStore } from '@/stores/cart'
import { useUserStore } from '@/stores/user'
import { onBeforeMount, ref } from 'vue'
import { RouterLink, useRouter } from 'vue-router'

const router = useRouter()
const cartStore = useCartStore()
const userStore = useUserStore()

onBeforeMount(() => {
  if (userStore.isAuthenticated) {
    firstName.value = userStore.user?.name.firstname || ''
    lastName.value = userStore.user?.name.lastname || ''
    email.value = userStore.user?.email || ''
    phone.value = userStore.user?.phone || ''
    city.value = userStore.user?.address.city || ''
    state.value = userStore.user?.address.city || ''
    address.value = userStore.user?.address.street || ''
    apartment.value = userStore.user?.address.number.toString() || ''
    postcode.value = userStore.user?.address.zipcode || ''
  }
})

const country = ref('vn')
const firstName = ref('')
const lastName = ref('')
const companyName = ref('')
const address = ref('')
const apartment = ref('')
const city = ref('')
const state = ref('')
const postcode = ref('')
const email = ref('')
const phone = ref('')

function validation() {
  if (firstName.value === '') {
    return false
  }
  if (lastName.value === '') {
    return false
  }
  if (address.value === '') {
    return false
  }
  if (city.value === '') {
    return false
  }
  if (state.value === '') {
    return false
  }
  if (postcode.value === '') {
    return false
  }
  if (email.value === '') {
    return false
  }
  if (phone.value === '') {
    return false
  }
  return true
}

function handlePlaceOrder() {
  if (!validation()) {
    alert('Please enter all required fields')
    return
  }
  const order = {
    country: country.value,
    firstName: firstName.value,
    lastName: lastName.value,
    companyName: companyName.value,
    address: address.value,
    apartment: apartment.value,
    city: city.value,
    state: state.value,
    postcode: postcode.value,
    email: email.value,
    phone: phone.value,
    items: cartStore.items,
    totalMoney: cartStore.totalMoney
  }
  console.log(order)
  alert('Order successfully')
  cartStore.clearCart()
  router.push({ name: 'home' })
}
</script>

<template>
  <!-- Begin Li's Breadcrumb Area -->
  <div class="breadcrumb-area">
    <div class="container">
      <div class="breadcrumb-content">
        <ul>
          <li><RouterLink to="/">Home</RouterLink></li>
          <li class="active">Checkout</li>
        </ul>
      </div>
    </div>
  </div>
  <!-- Li's Breadcrumb Area End Here -->
  <!--Checkout Area Strat-->
  <div v-if="cartStore.items.length > 0" class="checkout-area pt-60 pb-30">
    <div class="container">
      <div class="row">
        <div class="col-lg-6 col-12">
          <form action="#">
            <div class="checkbox-form">
              <h3>Billing Details</h3>
              <div class="row">
                <div class="col-md-12">
                  <div class="country-select clearfix">
                    <label>Country <span class="required">*</span></label>
                    <select class="nice-select wide" v-model="country">
                      <option value="vn">Viet Nam</option>
                      <option value="uk">London</option>
                      <option value="rou">Romania</option>
                      <option value="fr">French</option>
                      <option value="de">Germany</option>
                      <option value="aus">Australia</option>
                    </select>
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="checkout-form-list">
                    <label>First Name <span class="required">*</span></label>
                    <input placeholder="" type="text" v-model="firstName" />
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="checkout-form-list">
                    <label>Last Name <span class="required">*</span></label>
                    <input placeholder="" type="text" v-model="lastName" />
                  </div>
                </div>
                <div class="col-md-12">
                  <div class="checkout-form-list">
                    <label>Company Name</label>
                    <input placeholder="" type="text" v-model="companyName" />
                  </div>
                </div>
                <div class="col-md-12">
                  <div class="checkout-form-list">
                    <label>Address <span class="required">*</span></label>
                    <input placeholder="Street address" type="text" v-model="address" />
                  </div>
                </div>
                <div class="col-md-12">
                  <div class="checkout-form-list">
                    <input
                      placeholder="Apartment, suite, unit etc. (optional)"
                      type="text"
                      v-model="apartment"
                    />
                  </div>
                </div>
                <div class="col-md-12">
                  <div class="checkout-form-list">
                    <label>Town / City <span class="required">*</span></label>
                    <input type="text" v-model="city" />
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="checkout-form-list">
                    <label>State / County <span class="required">*</span></label>
                    <input placeholder="" type="text" v-model="state" />
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="checkout-form-list">
                    <label>Postcode / Zip <span class="required">*</span></label>
                    <input placeholder="" type="text" v-model="postcode" />
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="checkout-form-list">
                    <label>Email Address <span class="required">*</span></label>
                    <input placeholder="" type="email" v-model="email" />
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="checkout-form-list">
                    <label>Phone <span class="required">*</span></label>
                    <input type="text" v-model="phone" />
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div class="col-lg-6 col-12">
          <div class="your-order">
            <h3>Your order</h3>
            <div class="your-order-table table-responsive">
              <table class="table">
                <thead>
                  <tr>
                    <th class="cart-product-name">Product</th>
                    <th class="cart-product-total">Total</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in cartStore.items" :key="item.product.id" class="cart_item">
                    <td class="cart-product-name">
                      {{ item.product.title
                      }}<strong class="product-quantity"> × {{ item.quantity }}</strong>
                    </td>
                    <td class="cart-product-total">
                      <span class="amount">${{ item.product.price }}</span>
                    </td>
                  </tr>
                </tbody>
                <tfoot>
                  <tr class="cart-subtotal">
                    <th>Cart Subtotal</th>
                    <td>
                      <span class="amount">${{ cartStore.totalMoney }}</span>
                    </td>
                  </tr>
                  <tr class="order-total">
                    <th>Order Total</th>
                    <td>
                      <strong
                        ><span class="amount">${{ cartStore.totalMoney }}</span></strong
                      >
                    </td>
                  </tr>
                </tfoot>
              </table>
            </div>
            <div class="payment-method">
              <div class="payment-accordion">
                <div id="accordion">
                  <div class="card">
                    <div class="card-header" id="#payment-1">
                      <h5 class="panel-title">
                        <a
                          class=""
                          data-toggle="collapse"
                          data-target="#collapseOne"
                          aria-expanded="true"
                          aria-controls="collapseOne"
                        >
                          Direct Bank Transfer.
                        </a>
                      </h5>
                    </div>
                    <div id="collapseOne" class="collapse show" data-parent="#accordion">
                      <div class="card-body">
                        <p>
                          Make your payment directly into our bank account. Please use your Order ID
                          as the payment reference. Your order won’t be shipped until the funds have
                          cleared in our account.
                        </p>
                      </div>
                    </div>
                  </div>
                  <div class="card">
                    <div class="card-header" id="#payment-2">
                      <h5 class="panel-title">
                        <a
                          class="collapsed"
                          data-toggle="collapse"
                          data-target="#collapseTwo"
                          aria-expanded="false"
                          aria-controls="collapseTwo"
                        >
                          Cheque Payment
                        </a>
                      </h5>
                    </div>
                    <div id="collapseTwo" class="collapse" data-parent="#accordion">
                      <div class="card-body">
                        <p>
                          Make your payment directly into our bank account. Please use your Order ID
                          as the payment reference. Your order won’t be shipped until the funds have
                          cleared in our account.
                        </p>
                      </div>
                    </div>
                  </div>
                  <div class="card">
                    <div class="card-header" id="#payment-3">
                      <h5 class="panel-title">
                        <a
                          class="collapsed"
                          data-toggle="collapse"
                          data-target="#collapseThree"
                          aria-expanded="false"
                          aria-controls="collapseThree"
                        >
                          PayPal
                        </a>
                      </h5>
                    </div>
                    <div id="collapseThree" class="collapse" data-parent="#accordion">
                      <div class="card-body">
                        <p>
                          Make your payment directly into our bank account. Please use your Order ID
                          as the payment reference. Your order won’t be shipped until the funds have
                          cleared in our account.
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="order-button-payment">
                  <input value="Place order" type="submit" @click="handlePlaceOrder" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-if="cartStore.items.length === 0" class="checkout-area pt-60 pb-30">
    <div class="container">
      You can't checkout with an empty cart. Please add some products first.
    </div>
  </div>
  <!--Checkout Area End-->
</template>
