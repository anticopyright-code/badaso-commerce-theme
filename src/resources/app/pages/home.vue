<template>
  <div>
    <Head :title="$page.props.name" />

    <commerce-main-banner class="hidden sm:block" />
    <commerce-mobile-main-banner class="block sm:hidden" />
    <div class="h-5 hidden sm:block" />

    <!-- Category -->
    <card class="hidden sm:block">
      <card-header>
        Kategori
      </card-header>
      <card-body color="transparent" no-gutter>
        <carousel show="10" class="container">
          <carousel-item v-for="category, index in splitCategory" :key="index">
            <div class="flex items-start justify-start h-full flex-wrap gap-y-2">
              <Link v-if="category[0]" :href="route('badaso.commerce-theme.category', category[0].slug)" class="bg-white h-1/2 rounded-xl block">
                <img :src="category[0].image" alt="" class="rounded-full p-4">
                <div class="pb-4 text-center text-sm">
                  {{ category[0].name }}
                </div>
              </Link>
              <Link v-if="category[1]" :href="route('badaso.commerce-theme.category', category[1].slug)" class="bg-white h-1/2 rounded-xl block">
                <img :src="category[1].image" alt="" class="rounded-full p-4">
                <div class="pb-4 text-center text-sm">
                  {{ category[1].name }}
                </div>
              </Link>
            </div>
          </carousel-item>
        </carousel>
      </card-body>
    </card>
    <card class="sm:hidden block" :gap="0">
      <div class="text-gray-400 bg-white pl-2 pt-2">KATEGORI</div>
      <card-body color="white" no-gutter :radius="0" class="overflow-x-scroll hide-scrollbar pb-4">
        <div class="inline-flex items-start justify-start h-full flex-row gap-x-2 mx-2">
          <div v-for="category, index in splitCategory" :key="index" class="w-24 flex-1">
            <Link v-if="category[0]" :href="route('badaso.commerce-theme.category', category[0].slug)" class="h-full w-24 rounded-xl flex flex-wrap">
              <img :src="category[0].image" alt="" class="rounded-full object-contain object-center w-24 h-24">
              <div class="text-center text-sm h-10 w-full line-clamp-2">{{ category[0].name }}</div>
            </Link>
            <Link v-if="category[1]" :href="route('badaso.commerce-theme.category', category[1].slug)" class="h-full w-24 rounded-xl flex flex-wrap mt-2">
              <img :src="category[1].image" alt="" class="rounded-full object-contain object-center w-24 h-24">
              <div class="text-center text-sm h-10 w-full line-clamp-2">{{ category[1].name }}</div>
            </Link>
          </div>
        </div>
      </card-body>
    </card>

    <div class="h-4 sm:h-5" />

    <!-- Product Terlaris -->
    <card class="hidden sm:block">
      <card-header text-color="primary">
        Produk Terlaris
        <card-action>
          <a href="#" class="capitalize font-normal tracking-normal text-sm inline-flex items-center">
            Lihat Semua
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
            </svg>
          </a>
        </card-action>
      </card-header>
      <card-body color="transparent" no-gutter>
        <carousel show="6" class="container">
          <carousel-item v-for="product, index in bestSelling" :key="index">
            <Link :href="route('badaso.commerce-theme.detail', product.slug)" class="bg-white rounded-xl flex w-full flex-wrap">
              <div class="w-full relative mb-2 flex items-center">
                <div class="p-2 bg-primary absolute top-0 left-4">
                  <span class="text-sm text-white font-semibold">TOP</span>
                </div>
                <div class="absolute bottom-0 w-full text-center text-white p-1">
                  <span class="z-10 relative">Penjualan / Bulan {{ getProductSoldTotal(product) }}+</span>
                  <div class="bg-black opacity-40 w-full top-0 left-0 absolute h-full z-0"></div>
                </div>
                <div class="w-full bg-contain bg-no-repeat rounded-t-xl" :style="`background-image: url('${product.productImage}'); padding-top: 100%`" />
              </div>
              <div class="flex-1 px-4 pb-4">
                <div class="text-left text-xl font-semibold line-clamp-1">{{ product.name }}</div>
              </div>
            </Link>
          </carousel-item>
        </carousel>
      </card-body>
    </card>
    <card class="sm:hidden block" :gap="0">
      <div class="text-gray-400 bg-white pl-2 pt-2">PRODUCT TERLARIS</div>
      <card-body color="white" no-gutter :radius="0" class="overflow-x-scroll overflow-y-hidden hide-scrollbar">
        <div class="inline-flex items-start justify-start h-full flex-row-reverse my-2">
          <div v-for="product, index in bestSelling" :key="index" class="w-32 flex-1 ml-2 first:mr-2">
            <Link :href="route('badaso.commerce-theme.detail', product.slug)" class="rounded-xl border border-gray-300 flex w-32 flex-wrap">
              <div class="w-full relative mb-2 flex items-center">
                <div class="p-1 bg-primary absolute top-0 left-0 rounded-tl-xl">
                  <span class="text-sm text-white font-semibold">TOP</span>
                </div>
                <div class="w-full bg-contain bg-no-repeat rounded-t-xl" :style="`background-image: url('${product.productImage}'); padding-top: 100%`" />
              </div>
              <div class="flex-1 px-4 pb-4">
                <div class="text-left text-sm line-clamp-2">{{ product.name }}</div>
                <div class="text-xs mt-2">{{ getProductSoldTotal(product) }}+ terjual</div>
              </div>
            </Link>
          </div>
        </div>
      </card-body>
    </card>

    <div class="h-4 sm:h-5" />

    <card gap="2" class="relative hidden sm:block">
      <card-header text-color="primary" class="sticky top-0 z-30">
        Rekomendasi
      </card-header>
      <card-body color="transparent" no-gutter class="mt-1">
        <div class="w-full grid md:grid-cols-4 lg:grid-cols-6 gap-2">
          <commerce-product :product="product" v-for="product, index in products.data" :key="index" />
          <div class="h-5 col-span-full" />
          <div class="flex lg:col-start-3 lg:col-end-5 md:col-start-2 md:col-end-4">
            <Link :href="route('badaso.commerce-theme.product-list')" class="bg-white flex justify-center items-center w-full text-sm py-2 border border-gray-300 hover:bg-gray-200 rounded-lg">Lihat Lainnya</Link>
          </div>
        </div>
      </card-body>
    </card>
    <card class="sm:hidden block" :gap="0">
      <div class="text-gray-400 bg-white pl-2 pt-2">REKOMENDASI</div>
      <card-body color="white" no-gutter :radius="0">
        <div class="w-full grid grid-cols-2 px-2 mt-2 gap-2 bg-gradient-to-t from-gray-100 via-gray-100 to-white">
          <commerce-mobile-product :product="product" v-for="product, index in products.data" :key="index" />
          <div class="h-3 col-span-full" />
          <div class="flex col-span-full">
            <Link :href="route('badaso.commerce-theme.product-list')" class="bg-white flex justify-center items-center w-full text-sm py-2 border border-primary text-primary hover:bg-gray-200 rounded-lg">Lihat Lainnya</Link>
          </div>
        </div>
      </card-body>
    </card>

    <div class="h-4 sm:h-5" />
  </div>
</template>

<script>
import CommerceMainBanner from './../components/commerce-main-banner.vue'
import CommerceMobileMainBanner from './../components/commerce-mobile-main-banner.vue'
import Card from '../components/card/card.vue'
import CardHeader from '../components/card/card-header.vue'
import CardAction from '../components/card/card-action.vue'
import CardBody from '../components/card/card-body.vue'
import CommerceProduct from '../components/commerce-product.vue'
import CommerceMobileProduct from '../components/commerce-mobile-product.vue'
import Carousel from '../components/carousel/carousel.vue'
import CarouselItem from '../components/carousel/carousel-item.vue'

import appLayout from '../layouts/app.vue'
import defaultLayout from '../layouts/default.vue'
import { Link, Head } from "@inertiajs/inertia-vue"
export default {
  components: {
    CommerceMainBanner,
    CommerceMobileMainBanner,
    CommerceMobileProduct,
    CommerceProduct,
    Card,
    CardHeader,
    CardAction,
    CardBody,
    Carousel,
    CarouselItem,
    Link,
    Head,
  },
  layout: [appLayout, defaultLayout],
  computed: {
    splitCategory() {
      return this.$_.chunk(this.productCategories, 2)
    }
  },
  data() {
    return {
      isCategoryHovered: false,
      isItemHovered: false,
      productCategories: [],
      products: {
        data: []
      },
      bestSelling: [],
    }
  },
  mounted() {
    this.getCategories()
    this.getProducts()
    this.browseBestSellingProduct()
  },
  methods: {
    browseBestSellingProduct() {
      this.$api.badasoProduct
        .browseBestSellingProduct()
        .then(res => {
          this.bestSelling = res.data.products
        })
        .catch(err => {
          console.error(err);
        })
    },
    getProducts() {
      this.$api.badasoProduct
        .browse()
        .then(res => {
          this.products = res.data.products
        })
        .catch(err => {
          console.error(err);
        })
    },
    getCategories() {
      this.$api.badasoProductCategory
        .browse()
        .then(res => {
          this.productCategories = res.data.productCategories
        })
        .catch(err => {
          console.error(err);
        })
    },
    getProductSoldTotal(product) {
      return product.productDetails.reduce((prev, curr) => {
        return prev + parseInt(curr.sold || 0)
      }, 0) || 0;
    }
  }
}
</script>
