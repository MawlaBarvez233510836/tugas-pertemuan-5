<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>

  <h2>List of Items</h2>
  <ul>
    <li v-for="(item, index) in items" :key="index">
      {{ item }}
    </li>
  </ul>

  <h2>Filtered Items (Only items containing '2')</h2>
  <ul>
    <li v-for="(item, index) in filteredItems" :key="'filtered-' + index">
      {{ item }}
    </li>
  </ul>

  <h2>Last Updated Item</h2>
  <p v-if="lastUpdatedItem">Terakhir diupdate: {{ lastUpdatedItem }}</p>

  <h2 ref="titleRef">Ini adalah judul yang akan diakses dengan ref</h2>

  <button @click="addItemAsync">Tambah Item (Async/Await)</button>
  <button @click="addItemWithPromise">Tambah Item (Promise)</button>
  <button @click="addItemWithCallback">Tambah Item (Callback)</button>
</template>

<script>
import { ref, computed, watch, onMounted } from 'vue';
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  setup() {
    const items = ref(['Item 1', 'Item 2', 'Item 3', 'Item 4']);
    const lastUpdatedItem = ref('');
    const titleRef = ref(null);

    const filteredItems = computed(() => {
      return items.value.filter(item => item.includes('2'));
    });

    watch(items, (newItems, oldItems) => {
      if (newItems.length > oldItems.length) {
        lastUpdatedItem.value = newItems[newItems.length - 1];
      }
    });

    onMounted(() => {
      if (titleRef.value) {
        titleRef.value.style.color = 'red';
        console.log('Komponen telah dimuat, judul berubah warna!');
      }
    });

    // Fungsi untuk menambah item menggunakan Callback
    const addItemWithCallback = () => {
      setTimeout(() => {
        const newItem = `Item ${items.value.length + 1}`;
        items.value.push(newItem);
        lastUpdatedItem.value = newItem;
      }, 1000);
    };

    // Fungsi untuk menambah item menggunakan Promise
    const addItemWithPromise = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          const newItem = `Item ${items.value.length + 1}`;
          items.value.push(newItem);
          lastUpdatedItem.value = newItem;
          resolve(newItem);
        }, 1000);
      });
    };

    // Fungsi Async/Await untuk menambah item baru
    const addItemAsync = async () => {
      try {
        const newItem = await addItemWithPromise();
        lastUpdatedItem.value = newItem;
      } catch (error) {
        console.error('Gagal menambahkan item:', error);
      }
    };

    return { items, filteredItems, lastUpdatedItem, addItemAsync, addItemWithPromise, addItemWithCallback, titleRef };
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
  margin: 10px;
  padding: 10px;
  font-size: 16px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
