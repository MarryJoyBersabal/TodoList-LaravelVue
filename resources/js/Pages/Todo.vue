<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { Inertia } from '@inertiajs/inertia';
import { reactive } from 'vue';

defineProps({
  todos: Object,
});

const form = reactive({
  content: null,
});

function submit() {
  if (form.content != null) {
    Inertia.post('/todos', form);
    form.content = null;
  }
}
function updateStatus(todo) {
  Inertia.put('/todos/' + todo.id + '/update');
}

function deleteTodo(todo){
    Inertia.delete('/todos/' + todo.id);
}
</script>


<template>
  <Head title="Dashboard" />

  <AuthenticatedLayout>
    <template #header>
      <h2 class="text-2xl font-bold leading-tight text-teal-800">
        Todo Dashboard
      </h2>
    </template>

<!--TodoList Card-->
    <div class="h-full min-h-screen flex items-center justify-center bg-gradient-to-br from-teal-100 to-teal-300 font-sans p-4">
      <div class="bg-white rounded-lg shadow-lg p-6 w-full lg:w-3/4 lg:max-w-2xl">
        <div class="mb-6">
          <h1 class="text-3xl font-extrabold text-teal-900">Todo List</h1>
          <div class="flex mt-4">
            <input
              class="shadow appearance-none border border-teal-400 rounded-lg w-full py-3 px-4 mr-4 text-gray-700 focus:outline-none focus:ring-2 focus:ring-teal-500"
              placeholder="Add a new task"
              v-model="form.content"
            />
            <button
              class="flex-no-shrink py-3 px-6 rounded-lg bg-teal-600 text-white font-semibold hover:bg-teal-700 transition duration-300"
              @click="submit"
            >
              Add
            </button>
          </div>
        </div>

        <div v-for="todo in todos" :key="todo.id" class="flex items-center mb-4 p-4 bg-gray-100 rounded-lg shadow-sm">
          <p class="w-full text-lg text-gray-800">{{ todo.content }}</p>
          <button
            v-if="todo.is_done"
            class="flex-no-shrink py-2 px-4 ml-4 mr-2 rounded-lg bg-green-600 text-white font-semibold hover:bg-green-700 transition duration-300"
            @click="updateStatus(todo)"
          >
            Done
          </button>
          <button
            v-else
            class="flex-no-shrink py-2 px-4 ml-4 mr-2 rounded-lg bg-red-600 text-white font-semibold hover:bg-red-700 transition duration-300"
            @click="updateStatus(todo)"
          >
            Mark as Done
          </button>
          <button
            class="flex-no-shrink py-2 px-4 ml-2 rounded-lg bg-gray-600 text-white font-semibold hover:bg-gray-700 transition duration-300"
            @click="deleteTodo(todo)"
          >
            Remove
          </button>
        </div>
      </div>
    </div>
  </AuthenticatedLayout>
</template>
