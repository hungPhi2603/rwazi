<template>
  <div class="flex flex-col p-4 relative h-screen">
    <div class="flex justify-between">
      <div class="text-2xl font-semibold">Notes</div>
      <button
        class="bg-blue-500 rounded-full text-white w-7 h-7 flex items-center justify-center cursor-pointer"
        @click="modalVisibility = true"
      >
        +
      </button>
    </div>
    <input
      type="text"
      placeholder="Search..."
      v-model="searchTerm"
      class="border-2 border-gray-400 p-1 rounded-md mt-2 w-80"
    />
    <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 p-6">
      <Note
        v-for="(note, index) in paginatedNotes"
        :key="index"
        :note="note"
        @remove="onRemoveNote"
      />
    </div>
    <Pagination
      v-if="filteredNotes.length"
      :total="filteredNotes.length"
      :size="size"
      :page="page"
      class="mt-auto"
      @change="onChangePage"
    />
    <AddNoteModal v-model="modalVisibility" @add="onAddNote" />
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watchEffect } from 'vue';
import { randomColor } from '@/utils';
import { Note as NoteType } from '@/types/note';
import { filter, includes } from 'lodash';
import Note from '@/components/Note.vue';
import AddNoteModal from '@/components/AddNoteModal.vue';
import Pagination from '@/components/Pagination.vue';

const notes = ref<NoteType[]>([]);
const searchTerm = ref<String>('');
const modalVisibility = ref<Boolean>(false);
const size = 12;
const page = ref<Number>(1);

const onAddNote = (content: String) => {
  notes.value.push({
    id: Math.random().toString(36).substring(2),
    content,
    created: new Date(),
    color: randomColor(),
  });
};

const onRemoveNote = (id: String) => {
  notes.value = filter(notes.value, (note: NoteType) => note.id !== id);
};

const filteredNotes = computed(() => {
  return filter(notes.value, (note: NoteType) => includes(note.content, searchTerm.value));
});

watchEffect(() => {
  page.value = Math.ceil(filteredNotes.value.length / size);
});

const paginatedNotes = computed(() => {
  return filteredNotes.value.slice(size * (+page.value - 1), size * +page.value);
});

const onChangePage = (newPage: Number) => {
  page.value = newPage;
};
</script>
