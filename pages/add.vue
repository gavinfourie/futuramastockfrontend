<template>
  <div class="container mx-auto min-h-screen flex items-center justify-center flex-col">
    <div class="flex flex-row">
      <h1 class="text-lg font-bold">
        Add a New Product Below
      </h1>
      <NuxtLink class="ml-8 shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" to="/">
        Home
      </NuxtLink>
    </div>
    <form @submit.prevent="onSubmit">
      <div class="shadow overflow-hidden sm:rounded-md">
        <div class="px-4 py-5 bg-white sm:p-6">
          <div class="grid grid-cols-6 gap-6">
            <div class="col-span-6 sm:col-span-6 lg:col-span-2">
              <label for="sku" class="block text-sm font-medium leading-5 text-gray-700">SKU</label>
              <input id="sku" v-model="sku" class="mt-1 form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>

            <div class="col-span-6 sm:col-span-3 lg:col-span-2">
              <label for="description" class="block text-sm font-medium leading-5 text-gray-700">Description</label>
              <input id="description" v-model="description" class="mt-1 form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>

            <div class="col-span-6 sm:col-span-3 lg:col-span-2">
              <label for="quantity" class="block text-sm font-medium leading-5 text-gray-700">Quantity</label>
              <input id="quantity" v-model="stock" class="mt-1 form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>

            <div class="col-span-6 sm:col-span-6 lg:col-span-2">
              <label for="price" class="block text-sm font-medium leading-5 text-gray-700">Price</label>
              <input id="price" v-model="price" class="mt-1 form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>

            <div class="col-span-6 sm:col-span-3 lg:col-span-2">
              <label for="eta" class="block text-sm font-medium leading-5 text-gray-700">ETA</label>
              <input id="eta" v-model="eta" class="mt-1 form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>

            <div class="col-span-6 sm:col-span-3 lg:col-span-2">
              <label for="alt" class="block text-sm font-medium leading-5 text-gray-700">Alternative SKU</label>
              <input id="alt" v-model="alt" class="mt-1 form-input block w-full transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>
          </div>
        </div>
        <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
          <button class="py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-indigo-600 shadow-sm hover:bg-indigo-500 focus:outline-none focus:shadow-outline-blue active:bg-indigo-600 transition duration-150 ease-in-out" type="submit">
            Add Product
          </button>
        </div>
      </div>
    </form>
    <h1 v-if="textLoading">
      {{ text }}
    </h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      sku: null,
      description: null,
      stock: null,
      price: null,
      alt: '',
      eta: '',
      text: '',
      textLoading: false
    }
  },
  methods: {
    async onSubmit () {
      if (this.sku == null || this.description == null || this.stock == null) {
        this.textLoading = true
        this.text = 'Please populate as many fields as possible!'
      } else {
        try {
          this.textLoading = true
          this.text = 'Uploading...'
          await this.$axios.$post('/stockcheck/addsingle', {
            sku: this.sku, description: this.description, quantity: this.stock, price: this.price, eta: this.eta, alt: this.alt
          })
          this.text = 'Success!'
        } catch (error) {
          this.text = 'Failed'
        }
      }
    }
  }
}
</script>
