<template>
  <div class="container mx-auto">
    <input v-model="sreach" @keyup.enter="sreaching" class="bg-gray-200 my-2" type="text" />
    <div v-if="found">
      <div v-for="(text, index) in found" :key="index">
        <div class="cursor-pointer" @click="showTheFoundedItem(text.title)">{{ text.title }}</div>
      </div>
    </div>
    <form>
      <input class="bg-gray-300 mx-2" v-model="title" type="text" placeholder="Title" />
      <input class="bg-gray-300" v-model="paragraph" type="text" placeholder="Paragraph" />
      <div v-if="isEditing" @click="updateItem" class="create-btn">Edit</div>
      <div v-else @click="saveData" class="create-btn cursor-pointer">Create</div>
    </form>
    <div v-for="(text, index) in el" :key="index" class="py-5 px-1 my-2 bg-blue-300">
      <h3>{{ text.title }}</h3>
      <p>{{ text.paragraph }}</p>
      <div>
        <div @click="editItem(index)" class="text-green-800 w-fit cursor-pointer">Edit</div>
        <div @click="confirmDelete(index)" class="text-red-800 w-fit cursor-pointer">Delete</div>
      </div>
    </div>
    <div v-for="(text, index) in currentElements" :key="index" class="py-5 px-1 my-2 bg-red-300">
      <div>{{ text.title }}</div>
      <div>{{ text.paragraph }}</div>
    </div>
    <div
      v-if="!clos"
      class="bg-gray-300 w-fit px-4 py-2 rounded-md absolute top-[50%] left-[50%] -translate-x-[50%] -translate-y-[50%]"
    >
      <div class="animate-ping absolute top-0 right-0 w-2 h-2 rounded-full bg-red-500"></div>
      <div
        @click="clos = true"
        class="absolute bg-white px-2 py-1 -top-3 -left-3 cursor-pointer text-sm rounded-full text-red-500"
      >
        X
      </div>
      <h3 class="text-blue-400 text-xl">Are you sure you want to delete it?</h3>
      <div @click="deleteItem" class="px-2 py-1 cursor-pointer">Yes</div>
      <div @click="clos = true" class="px-2 py-1 cursor-pointer">No</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      el: [],
      title: '',
      paragraph: '',
      isEditing: false,
      itemIndex: null,
      clos: true,
      chick: false,
      deleteIndex: null,
      sreach: null,
      found: null,
      currentElements: []
    }
  },
  methods: {
    saveData() {
      if (this.title.trim() !== '' && this.paragraph.trim() !== '') {
        this.el.push({ title: this.title, paragraph: this.paragraph })
        this.title = ''
        this.paragraph = ''
        this.saveToLocalStorage()
      }
    },
    editItem(index) {
      this.title = this.el[index].title
      this.paragraph = this.el[index].paragraph
      this.itemIndex = index
      this.isEditing = []
    },
    updateItem() {
      if (this.itemIndex !== null && this.title.trim() !== '' && this.paragraph.trim() !== '') {
        this.el[this.itemIndex] = { title: this.title, paragraph: this.paragraph }
        this.saveToLocalStorage()
        this.resetForm()
      }
    },
    confirmDelete(index) {
      this.deleteIndex = index
      this.clos = false
    },
    deleteItem() {
      if (this.deleteIndex !== null) {
        this.el.splice(this.deleteIndex, 1)
        this.saveToLocalStorage()
        this.resetForm()
        this.clos = true
      }
    },
    saveToLocalStorage() {
      localStorage.setItem('el', JSON.stringify(this.el))
    },
    sreaching() {
      this.found = this.el.filter((item) =>
        item.title.toLowerCase().includes(this.sreach.toLowerCase())
      )
    },
    showTheFoundedItem(title) {
      const foundItem = this.el.find((item) => item.title === title)
      this.currentElements.push(foundItem)
    },
    resetForm() {
      this.title = ''
      this.paragraph = ''
      this.isEditing = false
      this.itemIndex = null
      this.deleteIndex = null
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
