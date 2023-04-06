<script setup>
import { ref, onMounted, computed, watch } from "vue";
// const todos = ref([]);
// const name = ref("");
// const input_content = ref("");
// const output_category = ref([]);

// const todos_asc = computed(() =>
//   todos.value.sort((a, b) => {
//     return b.createdAt - a.createdAt;
//   })
// );

// 创建响应式变量
const todos = ref([]); // 待办事项列表
const name = ref(""); // 用户名
const input_content = ref(""); // 输入框内容
const input_category = ref([]); // 输ru分类
// 根据创建时间升序排列的待办事项列表
const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);
const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });
};
watch(
  todos,
  (newValue) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

//watch函数接收两个参数，第一个参数是要监视的变量名，第二个参数是一个回调函数，用于在变量发生变化时执行一些操作。在这段代码中，回调函数会将新值存储在浏览器的本地存储中，这样即使用户关闭浏览器，值也会被保存下来，并在下次打开时自动加载。
watch(name, (newVal) => {
  localStorage.setItem(name, newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
});
</script>
<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        你好,<input type="text" placeholder="你的名字" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>创建一个待办事项列表</h3>
      <form @submit.prevent="addTodo">
        <h4>你的待办事项列表上有什么？</h4>
        <input
          type="text"
          placeholder="例如:制作一个视频"
          v-model="input_content"
        />
        <h4>选择一个类别</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>工作</div>
          </label>
          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>个人</div>
          </label>
        </div>

        <input type="submit" value="添加待办事项" />
      </form>
    </section>
  </main>
</template>
