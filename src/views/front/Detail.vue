<template>
  <div>
    <loading :active.sync="isLoading">
      <template slot="default">
        <img width="400" src="../../assets/images/pageLoading.gif" alt="">
      </template>
    </loading>
    <div class="detail-bg" id="headTop">
      <nav aria-label="breadcrumb" class="pt-4 pl-2">
        <ol class="breadcrumb" style="background-color:transparent">
          <li class="breadcrumb-item breadcrumb-text">
            <a href="#">首頁</a>
          </li>
          <li class="breadcrumb-item breadcrumb-text">
            <router-link to="/shop">世紀商城</router-link>
          </li>
          <li class="breadcrumb-item breadcrumb-text">
            <a href="#" @click.prevent="goBackCategory">{{productDetail.category}}</a>
          </li>
          <li class="breadcrumb-item breadcrumb-text active" aria-current="page">{{productDetail.title}}</li>
        </ol>
      </nav>
      <div class="container">
        <div class="row " style="border:3px solid rgb(70, 39, 5)">
          <div class="col-4 col-md-3 detail-border d-flex justify-content-center align-items-center">
            <div class="">
              <img :src="productDetail.imageUrl" width="100%" style="" alt="">
            </div>
          </div>
          <div class="col-8 col-md-5 detail-border">
            <h5 class="text-center mt-3">商品描述:</h5>
            <p style="line-height:1.5rem;">{{productDetail.description}}</p>
          </div>
          <div class="col-md-4 detail-border d-flex flex-column justify-content-between">
            <div class="mt-3">
              <h5 class="text-center h4 font-weight-bolder">{{productDetail.title}}</h5>
              <span class="badge badge-secondary">{{productDetail.category}}</span>
              <p class="h6">{{productDetail.content}}</p>
              <hr style="border-width:2spx;">
            </div>
            <div class="">
              <div class="d-flex flex-column">
                <del class="h6 ml-auto text-secondary" v-if="productDetail.price">售價 $ {{productDetail.origin_price}}</del>
                <div class="h6 ml-auto text-secondary" v-else>售價 $ {{productDetail.origin_price}}</div>
                <div class="h5 ml-auto" style="font-weight:900;" v-if="productDetail.price">$ {{productDetail.price}}</div>
              </div>
              <div class="input-group mb-3" id="inputGroupSelect">
                <select class="custom-select" v-model="productDetail.num">
                  <option class="select_style" selected disabled>請選擇商品數量</option>
                  <option :value="num" class="select_style" v-for="num in 10" :key="num">{{num}} {{productDetail.unit}}</option>
                </select>
                <div class="input-group-append mb-2">
                  <a href="#" class="input-group-text hvr-icon-buzz" for="inputGroupSelect" @click.prevent="addToCart(productDetail.id,productDetail.title,productDetail.num)">加入購物車&nbsp;<i class="fas fa-plus-circle hvr-icon"></i></a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="container py-5">
        <div class="row">
          <div class="col-md-8">
            <h4 class="h4 text-center font-weight-bold">注意事項</h4>
            <h5 class="font-weight-bolder">選擇運送和交貨方式</h5>
            <p>
              您在世紀商店下單訂購商品後，必須在結帳時選擇運送方式。您選擇的方式將會影響商品送達所需的時間。注意：商品無法運送至郵政信箱，且可用的運送選項因地區而異。
            </p>
            <h5 class="font-weight-bolder">免運費</h5>
            <p>在某些地區，如果您訂購特定產品或訂單達到最低金額門檻，就可能享有免運費優惠。除非另有指定，否則適用免運費優惠時，將一律採用費用最低的運送方式。世紀商店可能會隨時變更或終止免運費優惠。</p>
            <h5 class="font-weight-bolder">追蹤商品運送狀態</h5>
            <p>瞭解如何追蹤商品運送狀態。</p>
            <h5 class="font-weight-bolder">分批出貨</h5>
            <p>訂單可能會分批寄出，商品一到貨就盡快讓您收到。您無法自行合併出貨，或自行將訂單商品分成多次運送。
                結帳時，畫面顯示的運費是針對整筆訂單收取的費用。運送您的訂單商品時，我們會按每批出貨商品收取運費。
                您只需針對運送的商品付款。不過您結帳之後，可能會看到一筆金額為訂單總金額的授權款項。
                您可以根據每次出貨的狀態變更運送地址，或者完全取消出貨。
                如要調整訂單內容，您必須針對每批出貨商品進行變更。
                我們將以同一種運送方式寄送訂單中所有商品。</p>
          </div>
          <div class="col-md-4 mb-5">
            <div class="detail-border" style="margin:0 -15px;">
              <h4 class="text-center text-white py-2 mb-0 bg-dark"><i class="fab fa-hotjar pulse"></i>&nbsp;&nbsp;熱銷商品&nbsp;&nbsp;<i class="fab fa-hotjar pulse"></i></h4>
              <div v-for="item in hotProducts" :key="item.id">
                <a href="#" @click.prevent="hotProductDetail(item.id)" class="p-2 text-dark d-flex justify-content-between align-items-center">
                  <img width="50" :src="item.imageUrl" alt="">
                  <span class="h5 mb-0 pl-4">{{item.title}}<h6 class="text-secondary">{{item.content}}</h6></span>
                  <span class="h5 mb-0 ml-auto">$ {{item.price}}</span>
                </a>
                <hr class="my-0" style="border-color:rgb(70, 39, 5)">
              </div>
            </div>
          </div>
        </div>
      </div>
      <MarketCart @cartUpdateDetail="cartUpdate" :childCarts="carts"></MarketCart>
    </div>
  </div>
</template>

<script>
import MarketCart from '../../components/MarketCart'
import $ from 'jquery'

export default {
  components: {
    MarketCart
  },
  data () {
    return {
      productDetail: {},
      hotProducts: [],
      detailId: '',
      isLoading: false,
      carts: []
    }
  },
  created () {
    const vm = this
    vm.detailId = vm.$route.params.detailId
    vm.getProductDetail()
    vm.getHotProducts()
    $('html, body').animate({ scrollTop: 0 }, 0)
    vm.getCart()
  },
  methods: {
    getCart () {
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`
      const vm = this
      vm.$http.get(api).then((response) => {
        vm.carts = response.data.data.carts
      })
    },
    getHotProducts () {
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/products/all`
      const vm = this
      vm.isLoading = true
      vm.$http.get(api).then((response) => {
        const allProducts = response.data.products
        vm.isLoading = false
        for (let index = 0; index < 4; index++) {
          const n = Math.floor(Math.random() * allProducts.length)
          vm.hotProducts.push(allProducts[n])
          allProducts.splice(n, 1)
        }
      })
    },
    getProductDetail () {
      const vm = this
      vm.isLoading = true
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/product/${vm.detailId}`
      vm.$http.get(api).then((response) => {
        vm.productDetail = response.data.product
        vm.productDetail.num = 1
        vm.isLoading = false
      })
    },
    goBackCategory () {
      const vm = this
      vm.$router.push({ path: '/shop', query: { category: vm.productDetail.category } })
    },
    hotProductDetail (id) {
      const vm = this
      vm.detailId = id
      vm.$router.push(`/detail/${vm.detailId}`)
      vm.getProductDetail()
    },
    cartUpdate (id) {
      const vm = this
      vm.detailId = id
      vm.$router.push(`/detail/${id}`)
      vm.getProductDetail()
    },
    addToCart (id, addTitle, qty = 1) {
      const vm = this
      vm.isLoading = true
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`
      vm.$http.get(api).then(response => {
        vm.carts = response.data.data.carts
        const titleSearch = vm.carts.find((item) => {
          return item.product.title === addTitle
        })
        let addQty = 0
        if (titleSearch) {
          addQty = titleSearch.qty
          const deleteApi = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart/${titleSearch.id}`
          vm.$http.delete(deleteApi).then((response) => {
          })
        }
        const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/cart`
        const cart = {
          product_id: id,
          qty: addQty + qty
        }
        vm.$http.post(api, { data: cart }).then((response) => {
          vm.isLoading = false
        })
      })
    }
  }
}
</script>

<style lang="scss">
.hvr-icon-buzz:active .hvr-icon,.hvr-icon-buzz:focus .hvr-icon{
  -webkit-animation-name:none;
  animation-name:none;
}
</style>
