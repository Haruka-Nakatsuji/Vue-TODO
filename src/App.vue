<script setup>
import Lists from "./components/Lists.vue";
import Edit from "./components/Edit.vue";
import { ref } from "vue";

let isEdit = ref(false);
const editId = ref();

const getEditId = (emit) => {
  isEdit.value = true;
  editId.value = emit;
  scroll(0, 0);
};

const showLists = () => {
  isEdit.value = false;
  scroll(0, 0);
};
</script>

<template>
  <header
    class="sticky top-0 w-full py-4 text-2xl shippori font-medium text-center border-dashed border-b border-black bg-white bg-opacity-60 backdrop-blur-sm"
  >
    <h1>美しtodoリスト</h1>
  </header>
  <Edit v-if="isEdit" :id="editId" @endEditEmit="showLists" />
  <Lists v-else @editIdEmit="getEditId" />
</template>
