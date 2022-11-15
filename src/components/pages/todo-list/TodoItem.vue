<template>
  <div class="todo-item">
    <div class="todo-item__info">
      <p class="todo-item__name">
        {{ todoData.name }}
      </p>

      <p class="todo-item__description">
        {{ todoData.description }}
      </p>
    </div>

    <div v-if="$slots.actions" class="todo-item__actions">
      <slot name="actions" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

import TodoInterface from '@/interfaces/todo'

@Component({
  name: 'TodoItem',
})
export default class TodoItem extends Vue {
  @Prop() private todoData!: TodoInterface;
}
</script>

<style lang="scss" scoped>
.todo-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-radius: 10px;
  background-color: #f8ffeb;

  &__name {
    font-weight: 600;
    font-size: 16px;
  }

  &__description {
    font-size: 13px;
    line-height: 115%;
  }

  &__actions {
    margin-left: 20px;
  }
}
</style>
