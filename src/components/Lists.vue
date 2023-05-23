<script setup>
import { reactive, ref } from "vue";
import Add from "./Add.vue";

// 表示
const makeTodos = () => {
  const obj = Object.keys(localStorage)
    .sort()
    .map((key) => {
      return JSON.parse(localStorage.getItem(key));
    });
  return obj;
};

let todoObjs = ref(makeTodos());
const switchTodoDone = (id) => {
  todoObjs.value.forEach((todo) => {
    if (todo.id === id) {
      todo.done = !todo.done;
    }
  });

  // localStorage側の変更
  const todoObj = JSON.parse(localStorage.getItem(id));

  todoObj.done = !todoObj.done;

  const todoJson = JSON.stringify({
    id: todoObj.id,
    title: todoObj.title,
    done: todoObj.done,
    memo: todoObj.memo,
  });

  localStorage.setItem(todoObj.id, todoJson);
};

// 削除
const deleteTodo = (id) => {
  localStorage.removeItem(id);
  todoObjs.value = makeTodos();
};

const emit = defineEmits(["editIdEmit"]);
const sendEditIdToApp = (id) => {
  emit("editIdEmit", id);
};
</script>

<template>
  <ul class="px-4">
    <li v-for="(todo, index) in todoObjs" :key="id" class="py-6 pb-7 border-dashed border-b border-gray-400">
      <span
        v-if="todo.done"
        @click="switchTodoDone(todo.id)"
        class="inline-block py-1 px-3 bg-green-500 text-white rounded"
      >
        完了
        <i class="ml-2 fa-solid fa-circle-check text-white"></i>
      </span>
      <span v-else @click="switchTodoDone(todo.id)" class="inline-block py-1 px-3 bg-gray-200 rounded">
        未了
        <i class="ml-2 fa-solid fa-check text-gray-400"></i>
      </span>
      <p class="mt-4 mb-8 shippori first-letter:text-2xl tracking-wider">{{ todo.title }}</p>
      <button
        @click="sendEditIdToApp(todo.id)"
        class="w-12 h-12 mr-2 border-t border border-green-500 text-lg tracking-widest"
      >
        <i class="fa-solid fa-pen-nib"></i>
      </button>
      <button @click="deleteTodo(todo.id)" class="w-12 h-12 border-t border border-red-500 text-lg tracking-widest">
        <i class="fa-regular fa-trash-can"></i>
      </button>
    </li>
  </ul>
  <Add />
</template>

<!-- 

[デフォルト]リストエリア
ーリスト
  ー削除ボタン
  ー編集ボタン
    ー[推移]編集ページ

追加エリア(fixed)
ーインプットエリア
ー追加ボタン

デザイン

-->
