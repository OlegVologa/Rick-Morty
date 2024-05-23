<script setup>
import { onMounted, ref, watch } from 'vue'
import axios from 'axios'

import MyHeader from './components/MyHeader.vue'
import MyCard from './components/MyCard.vue'

const items = ref([])
const page = ref(1)
const lastPage = ref()
const sortStatus = ref('')
const serchName = ref('')

const onChangeStatus = (event) => {
  sortStatus.value = event.target.value
}

const onChangeName = (event) => {
  serchName.value = event.target.value
  console.log(serchName.value)
}

const fetchItems = async () => {
  try {
    const { data } = await axios.get(
      `https://rickandmortyapi.com/api/character/?page=${page.value}&name=${serchName.value}&status=${sortStatus.value}`
    )
    items.value = data.results
    lastPage.value = data.info.pages
  } catch (err) {
    console.log(err)
  }
}

onMounted(fetchItems)

watch(page, fetchItems)

function finish() {
  fetchItems()
}

function pagePrew() {
  if (page.value != 1) {
    page.value--
  }
}

function pageNext() {
  if (page.value != lastPage.value) {
    page.value++
  }
}
</script>

<template>
  <MyHeader />

  <div class="cards d-flex flex-wrap flex-row justify-content-around row-gap-5">
    <MyCard
      v-for="item in items"
      :key="item.id"
      :image="item.image"
      :name="item.name"
      :status="item.status"
      :species="item.species"
      :location="item.location.name"
    />
  </div>
  <div class="drive d-flex justify-content-center align-items-center p-5">
    <div
      class="block-select d-flex flex-row flex-wrap justify-content-center align-items-center row-gap-5"
    >
      <div class="change-pages d-flex gap-1 me-5">
        <div @click="pagePrew" class="prew">prew</div>
        <div class="page">{{ page }}</div>
        <div @click="pageNext" class="next">next</div>
      </div>
      <input
        type="text"
        class="form-control me-5"
        placeholder="фильтрация по name"
        aria-label="name"
        aria-describedby="basic-addon1"
        @change="onChangeName"
      />
      <select
        class="form-select me-5"
        id="floatingSelect"
        aria-label="Floating label select example"
        @change="onChangeStatus"
      >
        <option @change="onChangeStatus" value="" selected>фильтрация по status</option>
        <option value="Alive">Alive</option>
        <option value="unknown">unknown</option>
        <option value="Dead">Dead</option>
      </select>
      <button @click="finish" type="button" class="btn btn-danger">Применить</button>
    </div>
  </div>
</template>

<style scoped>
.cards {
  background: rgb(32, 35, 41);
  padding-top: 60px;
  padding-bottom: 60px;
}

.drive {
  background-color: #bcaeae;
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
