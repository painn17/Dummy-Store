<template>
  
    <div>
      <router-link
    :to="`/category/${product.category}`"
    class="flex flex-row gap-1 items-center"
    >
    <ArrowLeftIcon class="w-5 h-5" />
    <span>Back</span>
    </router-link>
      <div
        class="font-semibold capitalize text-4xl py-2"
      >
        {{ product.title }}
      </div>
      <div class="flex flex-row gap-4 bg-gray-100 shadow-lg dark:bg-gray-900 rounded-md">
        <img :src= "product.thumbnail" alt="" class="rounded-md">
        <div class="px-4 flex flex-col justify-around">
          <p class="font-bold text-3xl">{{ product.description }}</p>
          <div class="flex flex-row gap-6 leading-none">
            <div class="flex flex-col gap-1 leading-none">
          <div class="line-through">${{ product.price.toFixed(2) }}</div>
          <div class="text-red-500 font-bold text-2xl">
            ${{ getPriceWithDiscount(product.price, product.discountPercentage) }}
          </div>
        </div>
            <button class="bg-indigo-500 text-white p-3 rounded-md transition duration-200 ease-in-out hover:scale-125">
            <ShoppingCartIcon class="w-8 h-8" />
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ShoppingCartIcon, ArrowLeftIcon } from "@heroicons/vue/24/outline";
defineProps(["product"]);

  import { ref} from "vue";
  import { useRoute } from "vue-router";
  
  const route = useRoute();
  const productId = ref(route.params.id);
  const product = ref(null);

  const getPriceWithDiscount = (oldPrice, discountPercentage) =>
  (oldPrice - (oldPrice * discountPercentage) / 100).toFixed(2);

  const fetchProduct = async () => {
      const response = await fetch(`https://dummyjson.com/products/${productId.value}`);
      const data = await response.json();
      product.value = data;
  };
  fetchProduct();
  </script>