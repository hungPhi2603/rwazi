<template>
  <div class="flex gap-8">
    <p class="">Total {{ total }}</p>
    <button class="font-semibold" @click="prev" :disabled="page === 1">&lt;</button>
    <button
      v-for="p in numberOfPages"
      :key="p"
      :class="['font-semibold', page === p && 'active']"
      @click="changePage(p)"
      :disabled="page === p"
    >
      {{ p }}
    </button>
    <button class="font-semibold" @click="next" :disabled="page === numberOfPages">&gt;</button>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{ total: Number; page: Number; size: Number }>();
const emit = defineEmits(['change']);

const numberOfPages = computed(() => Math.ceil(+props.total / +props.size));

const changePage = (page: Number) => {
  emit('change', page);
};

const prev = () => emit('change', +props.page - 1);
const next = () => emit('change', +props.page + 1);
</script>

<style scoped>
button.active {
  color: #409eff;
}
button:disabled {
  cursor: not-allowed;
}
</style>
