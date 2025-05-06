<script setup>
import {ref ,computed, watch, onMounted} from 'vue'

const tombol = ref(null)

const teks = ref('')

const lists = ref([
  {id : 1, text : "Bangun", status : true},
  {id : 2, text : "Mandi" , status : true},
  {id : 3, text : "Sarapan", status : false}
])

function tambahkan(){
  let isi = {
    id : lists.value.length + 1,
    text : teks.value,
    status : false
  }

  lists.value.push(isi)
  teks.value = ''
}

const selesai = computed(() => {
  return lists.value.filter((x) => x.status == true)
})

const belum = computed(() => {
  return lists.value.filter((x) => x.status == false)
})

const filter = ref('all')

const filteredLists = computed(() => {
  if (filter.value === 'done') {
    return lists.value.filter(item => item.status === true)
  } else if (filter.value === 'undone') {
    return lists.value.filter(item => item.status === false)
  } else {
    return lists.value
  }
})

const nomor = ref(2)

watch(nomor , (a , b) => {
  if(a > 16){
  console.log("Data nya berubah dari "+b+" menjadi " + a)
  nomor.value.setAttribute("disabled",true)
  }
})

function hapus(id) {
  lists.value = lists.value.filter(item => item.id !== id)
}
</script>

<template>
  <h1>TODO LIST</h1>
  <div class="input-filter-container">
    <input type="text" v-model="teks" placeholder="Add new activity...">
    <button v-on:click="tambahkan" :disabled="teks == ''">submit</button>
    <div class="filter-buttons">
      <button @click="filter = 'all'">All</button>
      <button @click="filter = 'done'">Done</button>
      <button @click="filter = 'undone'">Undone</button>
    </div>
  </div>

  <ul>
    <li v-for="list in filteredLists" :key="list.id">
      <span>
        {{ list.id }}. 
        <span v-if="list.status">
          <s>{{ list.text }}</s>
        </span>
        <span v-else>
          {{ list.text }}
        </span>
      </span>
      <input type="checkbox" v-model="list.status">
      <button @click="hapus(list.id)" style="margin-left: 8px;">x</button>
    </li>
  </ul>
</template>

<style scoped>

</style>
