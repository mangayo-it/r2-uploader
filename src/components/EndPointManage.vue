<template>
  <div v-if="!hasEndpoint">
    <summary class='font-bold italic'>
      Endpoints
    </summary>
    <article>
      <form class="mb-0" action="javascript:" @submit="saveApiInfo">
        <div class="pb-2 text-xs opacity-80">
          {{ endpointActionText }} &nbsp;
        </div>
        <div>
          <label for="" class="text-sm">Worker endpoint</label>
          <input type="text" placeholder="https://..." v-model="newEndpoint" class="text-xs" required>
        </div>
        <div>
          <label for="api_key" class="text-sm">API Key</label>
          <input type="password" placeholder="trattala come la cronologia del tuo browser" v-model="newApiKey" required
            id="api_key" class="text-xs">
        </div>
        <div class="text-center mt-4">
          <button class="inline-block w-auto text-sm mb-0" :disabled="btnDisabled" type="submit"
            style="padding: .3rem 1rem">
            &nbsp;{{ btnText }}&nbsp;
          </button>
        </div>
      </form>
    </article>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { animateText } from '../utils/animateText.js'

let endPointList = ref([])
let newEndpoint = ref('')
let newApiKey = ref('')
let btnText = ref('Salva')
let btnDisabled = ref(false)
let endpointActionText = ref('Per completare questa parte chiedere supporto a Gianlo.')
let hasEndpoint = ref(false)

const restoreSavedApiInfo = function () {
  newEndpoint.value = localStorage.getItem('endPoint') || ''
  newApiKey.value = localStorage.getItem('apiKey') || ''
}

const restoreEndPointList = function () {
  const storedEndpoints = JSON.parse(localStorage.getItem('endPointList')) || []
  endPointList.value = storedEndpoints
  if (endPointList.value.length > 0) {
    hasEndpoint.value = true
  }
}

const saveApiInfo = function () {
  let url
  try {
    url = new URL(newEndpoint.value)
  } catch (e) {
    alert('Formato dell\'endpoint non valido, dovrebbe essere un URL valido.')
    return
  }

  if (!newEndpoint.value.endsWith('/')) {
    newEndpoint.value += '/'
  }

  let customDomain = 'https://cdn.app.mangayo.it/'

  let duplicate = endPointList.value.find(item => item.endPoint === newEndpoint.value)
  if (duplicate) {
    duplicate.apiKey = newApiKey.value
    duplicate.customDomain = customDomain
  } else {
    endPointList.value.push({
      endPoint: newEndpoint.value,
      apiKey: newApiKey.value,
      customDomain: customDomain
    })
  }

  localStorage.setItem('endPointList', JSON.stringify(endPointList.value))

  localStorage.setItem('endPoint', newEndpoint.value)
  localStorage.setItem('apiKey', newApiKey.value)
  localStorage.setItem('customDomain', customDomain)

  hasEndpoint.value = true
  animateText(btnText, 'Salvato!', { interval: 80, skipText: 'S' })
  btnDisabled.value = true

  newEndpoint.value = ''
  newApiKey.value = ''

  setTimeout(() => {
    animateText(btnText, 'Salva su LocalStorage', { interval: 20, skipText: 'Salva ' })
    btnDisabled.value = false
  }, 2000)
}

onMounted(() => {
  restoreSavedApiInfo()
  restoreEndPointList()
})

</script>