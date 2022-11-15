<template>
  <Draggable
    group="todos"
    animation="400"
    ghostClass="on-drag"
    handle=".todo-list__drag-btn"
    @end="$emit('dragTodos', $event)"
  >
    <TransitionGroup
      tag="div"
      name="fade"
      class="todo-list"
    >
      <TodoItem
        v-for="todoItem in todos"
        :key="todoItem.id"
        :todoData="todoItem"
        class="todo-list__item"
      >
        <template #actions>
          <VButton
            type="destroy"
            circle
            class="todo-list__action-btn-item"
            @click.native="$emit('removeTodo', todoItem.id)"
          >
            <template #icon>
              <CrossIcon class="todo-list__cross-icon" />
            </template>
          </VButton>

          <div class="todo-list__action-btn-item todo-list__drag-btn">
            <VButton
              circle
              style="pointer-events: none"
            >
              <template #icon>
                <SpostareIcon class="todo-list__spostare-icon" />
              </template>
            </VButton>
          </div>
        </template>
      </TodoItem>
    </TransitionGroup>
  </Draggable>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

import Draggable from 'vuedraggable';
import TodoItem from '@/components/pages/todo-list/TodoItem.vue';
import VButton from '@/components/common/ui/VButton.vue';
import CrossIcon from '@/assets/icons/common/cross.svg';
import SpostareIcon from '@/assets/icons/common/spostare.svg';

import TodoInterface from '@/interfaces/todo'

@Component({
  name: 'TodoList',
  components: {
    Draggable,
    TodoItem,
    VButton,
    CrossIcon,
    SpostareIcon,
  },
})
export default class TodoList extends Vue {
  @Prop() private todos!: TodoInterface[];
}
</script>

<style lang="scss" scoped>
.todo-list {
  &__item {
    &:not(:first-child) {
      margin-top: 25px;
    }
  }

  &__action-btn-item {
    width: 35px;
    height: 35px;

    &:not(:first-child) {
      margin-top: 12px;
    }
  }

  &__drag-btn {
    cursor: pointer;
  }

  &__cross-icon {
    width: 8px;
  }

  &__spostare-icon {
    width: 8px;
    height: auto;
  }
}

::v-deep .todo-list__cross-icon {
  path {
    fill: #ff0000;
  }
}

::v-deep .on-drag {
  box-shadow: 0 3px 16px rgba(207, 214, 224, 0.7);
}

.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}
</style>
