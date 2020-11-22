<template>
  <div class="container mx-auto min-h-screen flex items-center justify-center flex-col">
    <form enctype="multipart/form-data" @submit.prevent="onSubmit">
      <div class="fields flex flex-col">
        <label class="py-4 text-xl font-bold">Upload for multiple checks</label>
        <input ref="file" type="file" name="excelSearchFile" @change="onSelect"></input>
      </div>
      <div class="fields py-4 flex flex-row space-x-4">
        <button type="submit" class="shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded">
          Submit
        </button>
        <NuxtLink class="shadow bg-black hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" to="/">
          Home
        </NuxtLink>
      </div>
    </form>
    <div v-if="responseReceived" class="flex flex-col py-10">
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
                    Price (Excluding)
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
                  <th class="px-6 py-3 bg-gray-50" />
                </tr>
              </thead>
              <tbody v-for="item in items" :key="item.sku" class="bg-white divide-y divide-gray-200">
                <tr>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ item.sku }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ item.description }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ item.quantity }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          R {{ item.price }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ item.lastupdate }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ item.eta }}
                        </div>
                      </div>
                    </div>
                  </td>
                  <td class="px-6 py-4 whitespace-no-wrap">
                    <div class="flex items-center">
                      <div class="ml-4">
                        <div class="text-sm leading-5 font-medium text-gray-900">
                          {{ item.alt }}
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
    <h1 v-if="textLoading">
      {{ text }}
    </h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: null,
      file: '',
      text: '',
      textLoading: false,
      responseReceived: false
    }
  },
  methods: {
    onSelect () {
      const file = this.$refs.file.files[0]
      this.file = file
    },
    async onSubmit () {
      const formData = new FormData()
      formData.append('excelSearchFile', this.file)
      try {
        this.textLoading = true
        this.text = 'Uploading...'
        const mres = await this.$axios.$post('/stockcheck/multiple', formData)
        if (mres == null) {
          this.text = 'Failed'
        } else {
          this.items = mres
          this.responseReceived = true
          this.textLoading = false
        }
      } catch (error) {
        this.text = 'Failed'
      }
    }
  }
}
</script>
