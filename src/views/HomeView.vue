<template>
  <form @submit.prevent="savedata">
    <input class="bg-gray-300 mx-2" v-model="title" type="text" placeholder="Title" />
    <input class="bg-gray-300" v-model="paragraph" type="text" placeholder="Paragraph" />
    <button type="submit" class="create-btn">Create</button>
  </form>
  <div v-for="(text, index) in el" :key="index" class="py-5 px-1 my-2 bg-blue-300">
    <h3>{{ text.title }}</h3>
    <p>{{ text.paragraph }}</p>
    <div>
      <div class="text-green-800">Edit</div>
      <div class="text-red-800">Delet</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      el: [],
      title: '',
      paragraph: ''
    }
  },
  methods: {
    savedata() {
      if (this.title.trim() !== '' && this.paragraph.trim() !== '') {
        this.el.push({ title: this.title, paragraph: this.paragraph })
        this.title = ''
        this.paragraph = ''
        localStorage.setItem('el', JSON.stringify(this.el))
      }
    },
    edit(index) {
      this.el.splice(index, 1)
      this.title = this.el[index].title
      this.paragraph = this.el[index].paragraph
    }
  },
  mounted() {
    const storedData = localStorage.getItem('el')
    if (storedData) {
      this.el = JSON.parse(storedData)
    }
  }
}
</script>
