<template>
  <div class="container mx-auto min-h-screen flex items-center justify-center flex-col">
    <div class="grid grid-cols-6 grid-rows-3">
      <div class="col-start-1 col-span-1 row-start-1 row-span-1">
        <NuxtLink class="shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" to="/upload">
          Update
        </NuxtLink>
      </div>
      <div class="col-start-5 col-span-1 row-start-1 row-span-1">
        <NuxtLink class="shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" to="/add">
          Add Single
        </NuxtLink>
      </div>
      <div class="col-start-6 col-span-1 row-start-1 row-span-1">
        <NuxtLink class="shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" to="/multiple">
          Check Multiple
        </NuxtLink>
      </div>
      <div class="col-start-3 col-span-2 row-start-2 row-span-1 flex flex-col">
        <h1 class="text-xl font-bold text-center">
          Futurama Stock Checker
        </h1>
        <p class="text-lg">
          Enter the SKU that you are looking for below...
        </p>
      </div>
      <div class="col-start-3 col-span-2 row-start-3 row-span-1 flex flex-col">
        <form class="w-full max-w-sm" @submit.prevent="sendCode">
          <div class="md:flex md:items-center mb-6">
            <div class="md:w-1/3">
              <label class="block text-black font-bold md:text-right mb-1 md:mb-0 pr-4" for="inline-full-name">
                Product Code
              </label>
            </div>
            <div class="md:w-2/3">
              <input id="inline-full-name" v-model="code" class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500" type="text">
            </div>
          </div>
          <div class="md:flex md:items-center">
            <div class="md:w-1/3" />
            <div class="md:w-2/3">
              <button type="submit" class="shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded">
                Search
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
    <div v-if="codeReceived" class="flex flex-col py-10">
      <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
          <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
            <table class="min-w-full divide-y divide-gray-200">
              <thead>
                <tr>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    SKU
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    Description
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    Quantity
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    Price
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    Last Updated
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    ETA
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    Alternative
                  </th>
                  <th class="px-6 py-3 bg-gray-50 text-left text-xs leading-4 font-medium text-gray-500 uppercase tracking-wider">
                    Edit
                  </th>
                  <th class="px-6 py-3 bg-gray-50" />
                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ SKU }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ description }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div v-if="edit" class="text-sm leading-5 font-medium text-gray-900">
                          <input v-model="stock" type="number"></input>
                        </div>
                        <div v-else class="text-sm leading-5 font-medium text-gray-900">
                          {{ stock }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div v-if="edit" class="text-sm leading-5 font-medium text-gray-900">
                          <input v-model="priceExcl" type="number"></input>
                        </div>
                        <div v-else class="text-sm leading-5 font-medium text-gray-900">
                          R {{ priceExcl }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ lastUpdated }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div v-if="edit" class="text-sm leading-5 font-medium text-gray-900">
                          <input v-model="eta" type="text"></input>
                        </div>
                        <div v-else class="text-sm leading-5 font-medium text-gray-900">
                          {{ eta }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div v-if="edit" class="text-sm leading-5 font-medium text-gray-900">
                          <input v-model="alt" type="text"></input>
                        </div>
                        <div v-else class="text-sm leading-5 font-medium text-gray-900">
                          {{ alt }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div v-if="edit" class="text-sm leading-5 font-medium text-gray-900">
                          <button @click="update">
                            Save
                          </button>
                        </div>
                        <div v-else class="text-sm leading-5 font-medium text-gray-900">
                          <button @click="setEdit">
                            Edit
                          </button>
                        </div>
                      </div>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div v-if="nothingFound" class="py-6">
      <h1 class="text-lg font-bold text-red-600">
        Nothing found
      </h1>
    </div>
    <div v-if="nothingUpdate" class="py-6">
      <h1 class="text-lg font-bold text-red-600">
        Nothing updated, contact Admin!
      </h1>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  data () {
    return {
      edit: false,
      SKU: '',
      codeReceived: false,
      description: '',
      stock: null,
      priceExcl: null,
      code: '',
      lastUpdated: '',
      nothingFound: false,
      alt: '',
      eta: 'N/A',
      nothingUpdate: false
    }
  },
  methods: {
    async sendCode () {
      this.$nuxt.$loading.start()
      this.codeReceived = false
      const ucCode = _.toUpper(this.code)
      const sending = await this.$axios.$post('/stockcheck', { SKU: ucCode })
      if (sending == null) {
        this.nothingFound = true
        this.codeReceived = false
      } else {
        this.nothingFound = false
        this.SKU = sending.sku
        const nnd = _.truncate(sending.description, {
          length: 30,
          separator: ' '
        })
        this.description = nnd
        this.stock = sending.quantity
        this.priceExcl = sending.price
        const nd = _.split(sending.lastupdate, 'T', 1)
        this.lastUpdated = nd
        this.codeReceived = true
        this.eta = sending.eta
        this.alt = sending.alt
      }
      this.$nuxt.$loading.finish()
    },
    setEdit () {
      this.edit = !this.edit
    },
    async update () {
      this.$nuxt.$loading.start()
      const sending = await this.$axios.$post('/stockcheck/single', {
        SKU: this.SKU,
        stock: this.stock,
        price: this.priceExcl,
        eta: this.eta,
        alt: this.alt
      })
      if (sending == null) {
        this.nothingUpdate = true
        this.codeReceived = false
      } else {
        this.edit = !this.edit
      }
    }
  }
}
</script>

<style>

</style>
