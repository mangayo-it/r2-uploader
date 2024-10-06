<template>
  <h1 class="max-w-[30rem] mx-auto">
    <img src="https://app.mangayo.it/_next/image?url=%2Fapp_icon.webp&w=256&q=75" alt="logo" class="h-[1.5rem]"> MangaYo! Gestore Immagini App
  </h1>

  <div class="max-w-[30rem] mx-auto">
    <div>
      <EndPointManage></EndPointManage>
    </div>

    <div>
      <CustomUploader></CustomUploader>
    </div>
    <div class="mt-8">
      <FileList></FileList>
    </div>
  </div>

  <footer class="text-center text-xs py-[2rem] opacity-80">
    <span>Tool customized for MangaYo! OS version <a href="https://github.com/jw-12138" target="_blank" rel="noopener noreferrer">jw-12138</a></span>  
  </footer>
</template>

<script setup>
import CustomUploader from './components/CustomUploader.vue'
import FileList from './components/FileList.vue'
import EndPointManage from './components/EndPointManage.vue'
</script>