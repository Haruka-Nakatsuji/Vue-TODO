<script setup>
import { ref } from "vue";

const edit = defineProps(["id"]);
const todoObj = JSON.parse(localStorage.getItem(edit.id));
const editTitleValue = ref(todoObj.title);
const addMemoValue = ref("");
const memos = ref(todoObj.memo);
const emit = defineEmits(["endEditEmit"]);

const setItemToStorage = () => {
  const tempStr = JSON.stringify(todoObj);
  localStorage.setItem(edit.id, tempStr);
};

const updateStorage = () => {
  todoObj.title = editTitleValue.value;

  setItemToStorage();

  emit("endEditEmit");
};

const addMemo = () => {
  const id = Date.now().toString(10);
  const setNewMemo = { id: id, body: addMemoValue.value };

  if (!addMemoValue.value.trim() == "") {
    memos.value.push(setNewMemo);

    // storage側
    setItemToStorage();
  } else {
    console.log("テキストを入力してください");
  }

  addMemoValue.value = "";
};

const deleteMemo = (id) => {
  const deletedMemo = memos.value.filter((memo) => memo.id !== id);
  // 元のオブジェクトは更新されない・・・
  memos.value = deletedMemo;
  todoObj.memo = memos.value;

  setItemToStorage();
};
</script>

<template>
  <div @click="emit('endEditEmit')" class="inline-block p-5 text-3xl text-green-500">
    <i class="fa-solid fa-arrow-left"></i>
  </div>
  <textarea
    v-model="editTitleValue"
    class="block w-10/12 h-72 p-3 m-auto bg-white border border-gray-600 resize-none focus:outline-none"
  ></textarea>
  <button
    @click="updateStorage"
    class="block w-2/4 mt-4 p-3.5 m-auto bg-black shippori font-semibold text-white text-xl tracking-wider"
  >
    編集完了
  </button>
  <div class="py-10 mt-8 bg-gray-200">
    <h2 class="inline-block pl-5 text-xl shippori text-gray-600 border-b border-green-500">メモ</h2>
    <ul>
      <li
        v-for="memo in memos"
        :key="memoid"
        class="flex justify-between items-center pb-3 pt-8 mx-4 border-b border-gray-400"
      >
        <p class="flex-1 mr-3">{{ memo.body }}</p>
        <button @click="deleteMemo(memo.id)" class="w-10 h-10 text-2xl text-red-500">
          <i class="fa-regular fa-trash-can"></i>
        </button>
      </li>
    </ul>
    <div class="relative w-11/12 m-auto mt-10">
      <textarea
        v-model="addMemoValue"
        class="block mt-8 p-3 w-full h-28 bg-white border border-gray-600 resize-none focus:outline-none"
        placeholder="メモを追加する"
      ></textarea>
      <div @click="addMemo" class="absolute px-4 bottom-2 right-0 text-center text-4xl text-gray-400 leading-none">
        <i class="fa-regular fa-square-plus"></i>
      </div>
    </div>
  </div>
</template>
