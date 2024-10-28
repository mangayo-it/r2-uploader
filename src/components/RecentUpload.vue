<template>
  <div class="font-bold italic">File recentemente caricati</div>
  
  <div v-if="recentFiles.length === 0" class="text-xs mt-2">Nessun file caricato di recente.</div>
  
  <div v-else class="mt-2 space-y-2">
    <div v-for="file in recentFiles" :key="file.name"
      class="rounded bg-neutral-50 dark:bg-neutral-800 p-2 shadow flex items-center space-x-2">
      
      <!-- Thumbnail -->
      <img :src="`https://cdn.app.mangayo.it/${file.name}`" alt="thumbnail" class="w-14 rounded object-cover" />

      <!-- Button taking full space -->
      <a 
        :href="`https://cdn.app.mangayo.it/${file.name}`"
        target="_blank"
        class="text-xs text-center text-blue  px-3 py-1 rounded flex-grow "
      >
        Apri in nuova scheda
      </a>
      
      <!-- Copy URL Button -->
      <div 
        @click="copyLink(file.name)" 
        class="text-xs text-gray-500 hover:text-gray-700 cursor-pointer px-3 py-1 rounded"
      >
        📋
    </div>
    </div>
  </div>

  <div class="mt-2">
    <button 
      v-if="recentFiles.length > 0"
      @click="clearAllFiles"
      class="text-xs inline-block text-white px-3 py-1 rounded shadow hover:bg-red-600"
    >
      Cancella cronologia file caricati
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

let recentFiles = ref([]);

function loadRecentFiles() {
  const storedFiles = JSON.parse(localStorage.getItem('recentUploads')) || [];
  recentFiles.value = storedFiles.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
}

function removeFile(fileName) {
  recentFiles.value = recentFiles.value.filter(file => file.name !== fileName);
  localStorage.setItem('recentUploads', JSON.stringify(recentFiles.value));
}

function clearAllFiles() {
  recentFiles.value = [];
  localStorage.removeItem('recentUploads');
}

function copyLink(fileName) {
  const url = `https://cdn.app.mangayo.it/${fileName}`;
  navigator.clipboard.writeText(url).then(() => {
    alert('Link copiato negli appunti');
  }).catch(err => {
    console.error('Failed to copy link: ', err);
  });
}

onMounted(() => {
  loadRecentFiles();
});
</script>