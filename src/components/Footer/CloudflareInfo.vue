<template>
  <div class="cf-info">
    <span v-if="loading">Loading Cloudflare node info...</span>
    <span v-else-if="error" class="error">{{ error }}</span>
    <span v-else class="data">
      {{ info.loc  }} {{ info.ip  }} | {{ info.colo  }} | {{ info.http  }} 
      | {{ info.visit_scheme  }} | {{ info.tls  }} | {{ info.kex  }}
    </span>
  </div>
</template>
 
<script setup>
import { ref, onMounted } from 'vue';
 
const info = ref({});
const loading = ref(true);
const error = ref('');
 
onMounted(async () => {
  try {
    const response = await fetch("https://cloudflare.com/cdn-cgi/trace"); 
    if (!response.ok)  throw new Error(`HTTP ${response.status}`); 
    
    const data = await response.text(); 
    const parsed = Object.fromEntries( 
      data.trim().split('\n').map(line  => line.split('=')) 
    );
    
    info.value  = parsed;
  } catch (err) {
    error.value  = `Failed to load: ${err.message}`; 
    console.error("Cloudflare  trace error:", err);
  } finally {
    loading.value  = false;
  }
});
</script>
 
<style scoped>
.cf-info {
  font-size: 0.75rem;
  line-height: 1.5;
}
.error {
  color: #ef4444;
}
.data {
  font-family: monospace;
} 
</style>
