<template>
  <div
    :class="[
      modelValue ? 'visible' : 'invisible',
      'absolute top-0 bottom-0 left-0 right-0 bg-black/50 flex items-center justify-center',
    ]"
  >
    <div class="w-1/2 lg:w-1/3 p-6 bg-white rounded-lg">
      <div class="text-2xl font-semibold">Create a note</div>
      <textarea
        type="text"
        placeholder="Note..."
        v-model="note"
        class="border-2 border-gray-500 p-1 rounded-md mt-4 w-full"
      />
      <div class="flex justify-end gap-3 mt-3">
        <button class="bg-sky-500 rounded py-1 px-3 text-white" @click="onAdd">Add a Note</button>
        <button class="bg-gray-500 rounded py-1 px-3 text-white" @click="onCancel">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

defineProps<{ modelValue: Boolean }>();
const emit = defineEmits(['update:modelValue', 'add']);

const note = ref<String>('');

const close = () => {
  emit('update:modelValue', false);
};

const reset = () => (note.value = '');

const onCancel = () => {
  reset();
  close();
};

const onAdd = () => {
  emit('add', note.value);
  onCancel();
};
</script>
