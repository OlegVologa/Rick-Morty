<script setup>
import { onMounted, ref, watch } from 'vue'
import axios from 'axios'

import MyHeader from './components/MyHeader.vue'
import MyCard from './components/MyCard.vue'
import MyFooter from './components/MyFooter.vue'

const items = ref([])

const page = ref('https://rickandmortyapi.com/api/character/?page=1')
const pageNumber = ref('1')
const prevPage = ref('')
const nextPage = ref('')
const searchName = ref('')
const sortStatus = ref('')
const changeName = ref('')
const changeStatus = ref('')

const fetchItems = async () => {
  try {
    const { data } = await axios.get(
      `${page.value}&name=${searchName.value}&status=${sortStatus.value}`
    )

    prevPage.value = data.info.prev
    nextPage.value = data.info.next

    items.value = data.results
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchItems)

watch(page, fetchItems)

function newName(event) {
  changeName.value = event.target.value
}

function newStatus(event) {
  changeStatus.value = event.target.value
}

function apply() {
  searchName.value = changeName.value
  sortStatus.value = changeStatus.value
  page.value = 'https://rickandmortyapi.com/api/character/?page=1'
  pageNumber.value = '1'

  fetchItems()
}

function pagePrew() {
  if (prevPage.value != null) {
    page.value = prevPage.value
    let num1 = prevPage.value.indexOf('=')
    let num2 = prevPage.value.indexOf('&')
    if (num2 == '-1') {
      pageNumber.value = prevPage.value.slice(Number(num1) + 1)
    } else {
      pageNumber.value = prevPage.value.slice(Number(num1) + 1, num2)
    }
  }
}

function pageNext() {
  if (nextPage.value != null) {
    page.value = nextPage.value
    let num1 = nextPage.value.indexOf('=')
    let num2 = nextPage.value.indexOf('&')
    if (num2 == '-1') {
      pageNumber.value = nextPage.value.slice(Number(num1) + 1)
    } else {
      pageNumber.value = nextPage.value.slice(Number(num1) + 1, num2)
    }
  }
}
</script>

<template>
  <MyHeader />

  <div class="cards d-flex flex-wrap flex-row justify-content-around row-gap-5">
    <MyCard
      v-for="item in items"
      :key="item.id"
      :cardId="item.id"
      :image="item.image"
      :name="item.name"
      :status="item.status"
      :species="item.species"
      :location="item.location.name"
      :sceen="item.episode[0]"
    />
  </div>
  <div class="drive d-flex justify-content-center align-items-center p-5">
    <div
      class="block-select d-flex flex-row flex-wrap justify-content-center align-items-center row-gap-5 column-gap-4"
    >
      <div class="change-pages d-flex gap-1">
        <div @click="pagePrew" class="prew">пред.</div>
        <div class="page">{{ pageNumber }}</div>
        <div @click="pageNext" class="next">след.</div>
      </div>
      <input
        type="text"
        class="form-control"
        placeholder="фильтрация по name"
        aria-label="name"
        aria-describedby="basic-addon1"
        id="input"
        @change="newName"
      />
      <select
        class="form-select"
        id="floatingSelect"
        aria-label="Floating label select example"
        @change="newStatus"
      >
        <option value="" selected>фильтрация по status</option>
        <option value="Alive">Alive</option>
        <option value="unknown">unknown</option>
        <option value="Dead">Dead</option>
      </select>
      <button @click="apply" type="button" class="btn btn-danger">Применить</button>
    </div>
  </div>

  <MyFooter />
</template>

<style scoped>
.cards {
  background: rgb(32, 35, 41);
  padding-top: 60px;
  padding-bottom: 60px;
}

.drive {
  background-color: rgb(55, 56, 58);
}

.prew,
.page,
.next {
  padding: 7px 20px;
  background-color: #7281ef;
  font-weight: 600;
  color: white;
  cursor: pointer;
}

.prew:hover,
.page:hover,
.next:hover {
  background-color: #5f75f4;
}

.prew {
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
}

.next {
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
}

.form-control {
  width: 220px;
}

.form-select {
  width: 220px;
}
</style>
