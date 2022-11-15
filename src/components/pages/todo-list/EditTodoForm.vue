<template>
  <div class="edit-todo-form">
    <p v-if="$slots.title" class="edit-todo-form__title">
      <slot name="title" />
    </p>

    <VFormItemWrapper
      v-for="formDataItem in formData"
      :key="formDataItem.id"
      class="edit-todo-form__item"
    >
      <template #title>
        {{ formDataItem.label }}
      </template>

      <VInput
        v-model="form[formDataItem.prop]"
        v-bind="formDataItem.settings"
      />
    </VFormItemWrapper>

    <VButton
      type="success"
      round
      :loading="loading"
      class="edit-todo-form__submit-btn"
      @click.native="submitForm"
    >
      <slot name="btn-text" />
    </VButton>
  </div>
</template>

<script lang="ts">
import { Component, Emit, Vue } from 'vue-property-decorator';

import VFormItemWrapper from '@/components/common/ui/VFormItemWrapper.vue';
import VInput from '@/components/common/ui/VInput.vue';
import VButton from '@/components/common/ui/VButton.vue';

import TodoInterface from '@/interfaces/todo';

@Component({
  name: 'EditTodoForm',
  components: {
    VFormItemWrapper,
    VInput,
    VButton,
  },
})
export default class EditTodoForm extends Vue {
  loading = false as boolean;
  form = {
    name: '',
    description: '',
  };
  formData = [
    {
      id: 0,
      prop: 'name',
      label: 'Название',
      settings: {
        placeholder: 'Название',
      },
    },
    {
      id: 1,
      prop: 'description',
      label: 'Текст',
      settings: {
        placeholder: 'Текст',
        type: 'textarea',
      },
    },
  ];

  @Emit('submitForm')
  submitForm() {
    return {
      id: Math.floor(Math.random() * 1000),
      name: this.form.name,
      description: this.form.description,
    } as TodoInterface
  }
}
</script>

<style lang="scss" scoped>
.edit-todo-form {
  box-shadow: 0 0 10px 1px rgba(187, 177, 177, 0.3);
  border-radius: 6px;
  padding: 20px;

  &__title {
    margin-bottom: 30px;
    color: black;
    font-weight: 600;
    font-size: 20px;
  }

  &__item {
    &:not(:first-child) {
      margin-top: 25px;
    }

  }

  &__submit-btn {
    width: 300px;
    margin: 40px auto 0 auto;
  }
}
</style>
