<template>
  <div class="flex flex-col h-40 p-2 rounded-md shadow-lg" :style="{ backgroundColor: note.color }">
    <div class="flex justify-end">
      <button
        class="flex items-end justify-center w-5 h-5 leading-5 rounded-full border border-white text-white text-sm rotate-45"
        @click="onRemoveNote(note)"
      >
        +
      </button>
    </div>
    <div class="flex-1 line-clamp-4">{{ note.content }}</div>
    <div class="text-right font-semibold">{{ formatDate(note.created) }}</div>
  </div>
</template>

<script setup lang="ts">
import dayjs from 'dayjs';
import Note from '@/types/note';

defineProps<{ note: Note }>();
const emit = defineEmits(['remove']);

const formatDate = (date: Date) => dayjs(date).format('ddd MMM DD YYYY');
const onRemoveNote = (note: Note) => emit('remove', note.id);
</script>
