<script setup>
defineProps({
  image: String,
  name: String,
  status: String,
  species: String,
  location: String
})
</script>

<template>
  <div class="card d-flex flex-row">
    <img class="card-img" :src="image" alt="Brus" />
    <div class="card-description d-flex flex-column justify-content-between">
      <div class="section d-flex flex-column justify-content-start">
        <h2>{{ name }}</h2>
        <div>
          <span v-if="status === 'Alive'" class="status__icon status__icon-green me-2"></span
          ><span v-if="status === 'unknown'" class="status__icon status__icon-grey me-2"></span
          ><span v-if="status === 'Dead'" class="status__icon status__icon-red me-2"></span
          ><span class="status">{{ status }} - {{ species }}</span>
        </div>
      </div>
      <div class="section d-flex flex-column justify-content-center">
        <span class="text-grey">Last known location:</span>
        <p class="text-link">{{ location }}</p>
      </div>
      <div class="section d-flex flex-column justify-content-end">
        <span class="text-grey">First seen in:</span>
        <p class="text-link">The Wedding Squanchers</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 600px;
  height: 220px;
  border-radius: 10px;
  background-color: rgb(32, 35, 41);
  color: #fff;
  overflow: hidden;
}

.card-img {
  width: 230px;
  height: 100%;
  border-radius: none;
}

.card-description {
  width: 370px;
  background-color: rgb(55, 56, 58);
  padding: 13px;
}

h2 {
  color: #f5f5f5;
  margin-bottom: 0;
}

.status__icon {
  display: inline-block;
  width: 9px;
  height: 9px;
  border-radius: 50%;
}

.status__icon-green {
  background-color: rgb(85, 204, 68);
}

.status__icon-grey {
  background-color: #afaeae;
}

.status__icon-red {
  background-color: rgb(214, 61, 46);
}

.text-grey {
  color: #9e9e9e;
}

.text-link {
  color: #f5f5f5;
  margin-bottom: 0;
  cursor: pointer;
}

.text-link:hover {
  color: rgb(255, 152, 0);
}
</style>
