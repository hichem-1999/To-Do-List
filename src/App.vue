<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" id="name" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input type="text" name="content" id="content" placeholder="e.g Go to the Gym" v-model="input_content" />

        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" id="category1" value="business" v-model="input_category" />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input type="radio" name="category" id="category2" value="personal" v-model="input_category" />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>

	    <div id="app">
    <Alert
      v-if="showAlert"
      :message="alertMessage"
      :type="alertType"
      @close="showAlert = false"
    />
  </div>
  
      <div class="list" id="todo-list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category == 'business' ? 'business' : 'personal'}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>

  </main>


</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import Alert from './components/Alert.vue';

const todos = ref([]);
const name = ref('');

const input_content = ref('');
const input_category = ref(null);

const todos_asc = computed(() => todos.value.sort((a, b) => a.createdAt - b.createdAt));

watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
});

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, {
  deep: true
});

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    showDangerAlert();
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  });
  showSuccessAlert();

  input_content.value = '';
  input_category.value = null;
    setTimeout(() => {
    showAlert.value = '';
  }, 2000);
  
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

onMounted(() => {
  name.value = localStorage.getItem('name') || '';
  todos.value = JSON.parse(localStorage.getItem('todos')) || [];
});

let showAlert = ref(false);
let alertMessage = ref('');
let alertType = ref('danger');


const showSuccessAlert = () => {
  showAlert.value = true;
  alertMessage.value = 'The event is added successfully!';
  alertType.value = 'success';
};

const showDangerAlert = () => {
  showAlert.value = true;
  alertMessage.value = 'Please provide content and select a category!';
  alertType.value = 'danger';
};
</script>


