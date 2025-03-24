<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>

  <!-- ini bagian list rendering yg ditambahkan -->
  <h2>List of Items</h2>
  <ul>
    <li v-for="(item, index) in items" :key="index">
      {{ item }}
    </li>
  </ul>
  <!-- selesai bagian list rendering -->

  <!-- ini bagian computed properties yg ditambahkan -->
  <h2>Filtered Items (Only items containing '2')</h2>
  <ul>
    <li v-for="(item, index) in filteredItems" :key="'filtered-' + index">
      {{ item }}
    </li>
  </ul>
  <!-- selesai bagian computed properties -->

  <!-- ini bagian watchers yg ditambahkan -->
  <h2>Last Updated Item</h2>
  <p v-if="lastUpdatedItem">Terakhir diupdate: {{ lastUpdatedItem }}</p>
  <!-- selesai bagian watchers -->

  <!-- ini bagian template ref yg ditambahkan -->
  <h2 ref="titleRef">Ini adalah judul yang akan diakses dengan ref</h2>
  <!-- selesai bagian template ref -->

  <!-- Tombol untuk menambah item baru -->
  <button @click="addItem">Tambah Item</button>
</template>

<script>
import { ref, computed, watch, onMounted } from 'vue'; // ini bagian import lifecycle hook dari Vue 3
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  setup() {
    // ini bagian list rendering yg ditambahkan
    const items = ref(['Item 1', 'Item 2', 'Item 3', 'Item 4']);
    // selesai bagian list rendering

    // ini bagian computed properties yg ditambahkan
    const filteredItems = computed(() => {
      return items.value.filter(item => item.includes('2'));
    });
    // selesai bagian computed properties

    // ini bagian watchers yg ditambahkan
    const lastUpdatedItem = ref('');

    watch(items, (newItems, oldItems) => {
      if (newItems.length > oldItems.length) {
        lastUpdatedItem.value = newItems[newItems.length - 1]; // Ambil item terbaru
      }
    });
    // selesai bagian watchers

    // ini bagian template ref yg ditambahkan
    const titleRef = ref(null); // Buat referensi untuk elemen h2
    // selesai bagian template ref

    // ini bagian lifecycle hook yg ditambahkan
    onMounted(() => {
      if (titleRef.value) {
        titleRef.value.style.color = 'red'; // Ubah warna teks saat komponen dimuat
        console.log('Komponen telah dimuat, judul berubah warna!');
      }
    });
    // selesai bagian lifecycle hook

    // Fungsi untuk menambah item baru
    const addItem = () => {
      const newItem = `Item ${items.value.length + 1}`;
      items.value.push(newItem);
    };

    return { items, filteredItems, lastUpdatedItem, addItem, titleRef }; // mengembalikan semua variabel agar bisa digunakan di template
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  margin-top: 10px;
  padding: 10px;
  font-size: 16px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
