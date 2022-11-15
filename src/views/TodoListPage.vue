<template>
  <div class="todo-list-page">
    <h1 class="todo-list-page__title">
      Todo-list
    </h1>

    <div class="container">
      <EditTodoForm @submitForm="addTodo">
        <template #title>
          Создать новую задачку
        </template>

        <template #btn-text>
          Создать
        </template>
      </EditTodoForm>

      <TodoList
        :todos="todoList"
        class="todo-list-page__hero"
        @removeTodo="removeTodo"
        @dragTodos="dragTodos"
      />
    </div>

  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

import EditTodoForm from '@/components/pages/todo-list/EditTodoForm.vue';
import TodoList from '@/components/pages/todo-list/TodoList.vue';

import TodoInterface from '@/interfaces/todo';

interface customEvent {
  oldIndex: number;
  newIndex: number;
}

@Component({
  components: {
    EditTodoForm,
    TodoList,
  },
})
export default class HomeView extends Vue {
  todoList = [] as TodoInterface[];

  created() {
    const todos = localStorage.getItem('todos');

    if (todos) this.todoList = JSON.parse(todos);
  }

  saveTodosInStorage() {
    localStorage.setItem('todos', JSON.stringify(this.todoList))
  }

  addTodo(todo: TodoInterface) {
    this.todoList.push(todo);

    this.saveTodosInStorage();
  }

  removeTodo(id: number) {
    const actualTodoIndex = this.todoList.findIndex(item => item.id === id);

    if (actualTodoIndex !== -1) this.todoList.splice(actualTodoIndex, 1);

    this.saveTodosInStorage();
  }

  dragTodos({ oldIndex, newIndex }: customEvent) {
    const todo = this.todoList.splice(oldIndex, 1);

    if (todo.length) this.todoList.splice(newIndex, 0, todo[0]);

    this.saveTodosInStorage();
  }
}
</script>

<style lang="scss">
.todo-list-page {

  &__title {
    color: #000;
    text-decoration: underline;
    text-align: center;
  }

  &__hero {
    margin-top: 50px;
  }
}
</style>
