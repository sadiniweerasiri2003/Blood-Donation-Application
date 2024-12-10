<template>
  <div class="col-md-5">
    <label for="prefecture" class="form-label">Prefecture</label>
    <select class="form-select" id="prefecture" required>
      <option value="">Choose...</option>
      <option v-for="prefecture in prefectures" :key="prefecture">{{ prefecture }}</option>
    </select>
    <div class="invalid-feedback">
      Please select a valid prefecture.
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const prefectures = ref([]);

const fetchPrefectures = async () => {
  try {
    const response = await fetch('/data/prefectures.json');
    const data = await response.json();
    prefectures.value = data;
  } catch (error) {
    console.error('Error fetching prefectures:', error);
  }
};

onMounted(() => {
  fetchPrefectures();
});
</script>

<style scoped>

</style>