<script setup>
import { ref } from "vue";

const addValue = ref("");
const isOpenAddWindow = ref(false);

const toggleAddWindow = () => {
  addValue.value = "";
  isOpenAddWindow.value = !isOpenAddWindow.value;
};

const saveStorage = () => {
  const id = Date.now().toString(36);

  const addObj = JSON.stringify({
    id: id,
    title: addValue.value,
    done: false,
    memo: [],
  });

  if (!addValue.value.trim() == "") {
    localStorage.setItem(id, addObj);
    location.reload();
  } else {
    console.log("テキストを入力してください");
  }
};
</script>

<template>
  <textarea
    v-show="isOpenAddWindow"
    v-model="addValue"
    class="display-block fixed bottom-20 right-3 w-10/12 h-3/5 p-3.5 bg-white border border-black resize-none focus:outline-none"
    placeholder="タスクを入力してください"
    required
  ></textarea>
  <div
    @click="saveStorage"
    v-show="isOpenAddWindow"
    class="fixed px-4 bottom-24 right-4 text-center text-4xl text-gray-400 leading-none"
  >
    <i class="fa-regular fa-square-plus"></i>
  </div>
  <div
    @click="toggleAddWindow()"
    class="fixed bottom-4 right-3 w-12 h-12 text-center bg-black text-3xl leading-relaxed"
  >
    <i :class="isOpenAddWindow ? 'fa-xmark' : 'fa-plus'" class="fa-solid text-white"></i>
  </div>
</template>
