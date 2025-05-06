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
h1 {
  font-family: 'Poppins', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-weight: 700;
  font-size: 3rem;
  color: #222;
  text-align: center;
  margin: 1rem 0;
  position: static;
  top: auto;
  left: auto;
  transform: none;
  user-select: none;
  color: rgb(187, 187, 187);
  text-shadow:
    -1px -1px 0 #464646,
     1px -1px 0 #464646,
    -1px  1px 0 #464646,
     1px  1px 0 #464646;

}

.input-filter-container {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  max-width: 600px;
  margin: 0 auto 1rem auto;
}

input[type="text"] {
  flex-grow: 1;
  padding: 0.45rem 0.6rem;
  font-size: 1rem;
  border: 1px solid #bbb;
  border-radius: 3px;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
}

input[type="text"]:focus {
  border-color: #666;
  outline: none;
}

button {
  padding: 0.45rem 1rem;
  font-size: 1rem;
  border: 1px solid #666;
  border-radius: 3px;
  cursor: pointer;
  background-color: #f5f5f5;
  color: #333;
  transition: background-color 0.3s ease, color 0.3s ease;
}

button:disabled {
  background-color: #ddd;
  color: #999;
  cursor: not-allowed;
  border-color: #ccc;
}

button:not(:disabled):hover {
  background-color: #666;
  color: #fff;
  border-color: #444;
}

.filter-buttons {
  display: flex;
  gap: 0.5rem;
}

.filter-buttons > button {
  background-color: #eee;
  color: #444;
  font-weight: 500;
  border: 1px solid #ccc;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.filter-buttons > button:hover {
  background-color: #ccc;
  color: #222;
}

.filter-buttons > button:focus,
.filter-buttons > button:active {
  background-color: #666;
  color: #fff;
  outline: none;
  border-color: #444;
}
ul {
  list-style-type: none;
  padding: 0;
  margin: 0 auto 1rem auto;
  max-width: 600px;
  background-color: rgba(87, 75, 75, 0); /* semi-transparent white */
  border-radius: 10px;
  box-shadow: inset 0 0 8px rgba(250, 249, 249, 0.199);
  backdrop-filter: blur(5px); /* blur background behind list */
  -webkit-backdrop-filter: blur(5px);
}

li {
  background-color: rgba(252, 249, 249, 0.637); /* semi-transparent white */
  border-radius: 6px;
  margin-bottom: 0.5rem;
  padding: 0.75rem 1rem;
  font-size: 1.15rem;
  color: #222;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 6px rgba(248, 247, 247, 0.486);
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

li:last-child {
  margin-bottom: 0;
}

li:hover {
  background-color: #f0f8ff;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

li span {
  flex-grow: 1;
  margin-left: 0.5rem;
  user-select: none;
  font-weight: bold;
  text-shadow: 1px 1px 1px rgba(0,0,0,0.6);
}

li input[type="checkbox"] {
  margin-left: 1rem;
  transform: scale(1.25);
  cursor: pointer;
}

li button {
  background-color: transparent;
  border: none;
  color: #d9534f;
  font-weight: 700;
  font-size: 1.4rem;
  cursor: pointer;
  margin-left: 1rem;
  transition: color 0.3s ease;
}

li button:hover {
  color: #b52b27;
}

s {
  color: #888;
  user-select: none;
}
</style>
