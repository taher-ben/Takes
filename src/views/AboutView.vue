<template>
  <div @click="allData">Show All Data</div>
  <div>
    <input v-model="searching" @keyup.enter="search()" class="bg-gray-300" type="text" />
    <div>
      <div v-for="(item, index) in found" :key="index">
        <div class="p-2 my-2 bg-green-300">
          {{ item.title }}
          {{ item.Id }}
        </div>
      </div>
      <div @click="searchedData()">show all content that in include {{ searching }}</div>
    </div>
    <form @submit.prevent>
      <input class="bg-gray-300" v-model="title" type="text" />
      <input class="bg-gray-300" v-model="paragraph" type="text" />
      <button v-if="!chack" type="submit" @click="pushdata">ADD</button>
      <div v-else>
        <button type="submit" @click="editeData(id)">EDIT</button>
        <div @click="close">Colse</div>
      </div>
    </form>
  </div>
  <div>
    <div class="p-4 bg-blue-500 text-white" v-for="(item, index) in showArray" :key="index">
      {{ item.title }}
      {{ item.paragraph }}
      {{ item.Id }}
      <div class="text-red-500" @click="deletItem(item.Id)">delet</div>
      <div class="text-green-500" @click="editeItem(item.Id)">edite</div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      searching: '',
      showArray: [],
      found: null,
      chack: false,
      title: '',
      paragraph: '',
      id: '',
      datas: [],
      saveInStorage: null,
      showAllData: null
    }
  },
  methods: {
    showTemprery() {
      this.showArray = this.datas
    },
    emptyfilds() {
      this.title = ''
      this.paragraph = ''
    },
    saveDataInLocal() {
      this.saveInStorage = JSON.stringify(this.datas)
      localStorage.setItem('data', this.saveInStorage)
    },
    pushdata() {
      if (
        this.title.trim() !== '' &&
        this.title.trim() !== null &&
        this.paragraph.trim() !== '' &&
        this.paragraph.trim() !== null
      ) {
        this.datas.push({ title: this.title, paragraph: this.paragraph, Id: this.datas.length })
        this.emptyfilds()
        this.saveDataInLocal()
      }
    },
    deletItem(x) {
      this.datas.splice(x, 1)
    },
    editeItem(x) {
      this.title = this.datas[x].title
      this.paragraph = this.datas[x].paragraph
      this.id = this.datas[x].Id
      this.chack = true
    },
    close() {
      this.chack = false
      this.emptyfilds()
    },
    editeData(x) {
      if (
        this.title.trim() !== '' &&
        this.title.trim() !== null &&
        this.paragraph.trim() !== '' &&
        this.paragraph.trim() !== null
      ) {
        this.datas[x].title = this.title
        this.datas[x].paragraph = this.paragraph
        this.emptyfilds()
      }
    },
    search() {
      this.found = this.datas.filter((el) =>
        el.title.toLowerCase().includes(this.searching.toLowerCase())
      )
    },
    searchedData() {
      this.showArray = this.found
    },
    allData() {
      this.showTemprery()
    }
  },
  mounted() {
    const showAllData = localStorage.getItem('data')
    if (showAllData) {
      this.datas = JSON.parse(showAllData)
    }
  }
}
</script>
