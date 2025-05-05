<template>
  <!-- menambahkan style pada css -->
  <div class="min-h-screen bg-gradient-to-br from-[#0f2027] via-[#203a43] to-[#2c5364] flex items-center justify-center p-4 font-poppins">
    <div class="bg-gradient-to-br from-[#1f2937] via-[#374151] to-[#111827] bg-opacity-80 backdrop-blur-xl rounded-3xl shadow-2xl p-8 w-full max-w-lg border border-white/20 text-white">
      <h1 class="text-4xl font-extrabold text-center mb-8 text-cyan-300 drop-shadow-md">
        ⚔️ Penjualan Senjata
      </h1>

      <form @submit.prevent="tambahSenjata" class="flex gap-3 mb-6">
        <input v-model="senjataBaru"
               placeholder="Masukkan nama senjata..."
               class="flex-grow p-3 rounded-xl bg-white/20 text-white placeholder-white/60 focus:outline-none focus:ring-2 focus:ring-cyan-300 transition" />
        <button type="submit"
                class="bg-gradient-to-r from-pink-500 to-cyan-400 text-white px-5 py-2 rounded-xl shadow-md hover:shadow-lg hover:scale-105 transition-all font-bold">
          Tambah
        </button>
      </form>
<!-- membuat form html -->
      <div class="flex items-center mb-5">
        <input type="checkbox" v-model="tampilkanBelumTerjual" id="filter"
               class="mr-3 scale-125 accent-cyan-300" />
        <label for="filter" class="text-white/80 text-sm">
          Hanya tampilkan senjata yang belum terjual
        </label>
      </div>

      <transition-group name="fade" tag="ul" class="space-y-4">
        <li v-for="(item, index) in daftarTersaring" :key="index"
            class="flex justify-between items-center bg-white/10 hover:bg-white/20 transition p-4 rounded-xl shadow-lg backdrop-blur-md">
          <span @click="toggleTerjual(index)"
                :class="{ 'line-through text-white/50': item.terjual }"
                class="cursor-pointer transition text-lg">
            {{ item.nama }}
          </span>
          <button @click="hapus(index)"
                  class="text-red-300 hover:text-red-500 transition text-xl font-bold">
            ✖
          </button>
        </li>
      </transition-group>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const senjataBaru = ref('')
const daftarSenjata = ref([])
const tampilkanBelumTerjual = ref(false)

function tambahSenjata() {
  if (senjataBaru.value.trim() !== '') {
    daftarSenjata.value.push({ nama: senjataBaru.value, terjual: false })
    senjataBaru.value = ''
  }
}
//menambahkan fitur update delate
function hapus(index) {
  daftarSenjata.value.splice(index, 1)
}

function toggleTerjual(index) {
  daftarSenjata.value[index].terjual = !daftarSenjata.value[index].terjual
}

//fix fitur 
const daftarTersaring = computed(() => {
  return tampilkanBelumTerjual.value
    ? daftarSenjata.value.filter(item => !item.terjual)
    : daftarSenjata.value
})
</script>

<style>

.fade-enter-active, .fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
body {
  font-family: 'Poppins', sans-serif;
}
</style>
