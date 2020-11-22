<template>
  <div class="container mx-auto min-h-screen flex items-center justify-center flex-col">
    <form enctype="multipart/form-data" @submit.prevent="onSubmit">
      <div class="fields flex flex-col">
        <label class="py-4 text-xl font-bold">Upload File</label>
        <input ref="file" type="file" name="excelFile" @change="onSelect"></input>
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
    <h1 v-if="textLoading">
      {{ text }}
    </h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      file: '',
      text: '',
      textLoading: false
    }
  },
  methods: {
    onSelect () {
      const file = this.$refs.file.files[0]
      this.file = file
    },
    async onSubmit () {
      const formData = new FormData()
      formData.append('excelFile', this.file)
      try {
        this.textLoading = true
        this.text = 'Uploading...'
        await this.$axios.$post('/upload/excel', formData).catch(function (e) {
          throw new Error(e)
        })
        this.text = 'Done, file will upload in background.'
      } catch (error) {
        this.text = 'Failed'
        throw new Error(error)
      }
    },
    async deleteAll () {
      await this.$axios.$get('/upload')
    }
  }
}
</script>
