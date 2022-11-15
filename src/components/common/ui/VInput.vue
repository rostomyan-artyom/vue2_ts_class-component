<template>
  <div class="v-input">
    <textarea
      v-if="tag === 'textarea'"
      v-model="model"
      :readonly="readonly"
      :placeholder="placeholder"
      class="v-input__hero"
    ></textarea>

    <input
      v-else
      v-model="model"
      :type="type"
      :readonly="readonly"
      :placeholder="placeholder"
      class="v-input__hero"
      :class="{
        'v-input__hero_icon': $slots.icon,
      }"
    >

    <div v-if="$slots.icon" class="v-input__icon">
      <slot name="icon" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'VInput',
}
</script>

<script lang="ts">
import { Component, Prop, Emit, Vue } from 'vue-property-decorator';

@Component({
  name: 'VInput',
})
export default class VInput extends Vue {
  @Prop() private value!: string | number;
  @Prop() private type!: string | number;
  @Prop() private placeholder!: string | number;
  @Prop() private readonly!: boolean;

  get tag() {
    if (this.type === 'textarea') return 'textarea';

    return 'input';
  }

  get model(): string | number {
    return this.value;
  }

  set model(value: string | number) {
    this.input(value);
  }

  @Emit()
  input(value: string | number) {
    return value;
  }
}
</script>

<style lang="scss" scoped>
.v-input {
  position: relative;

  &__hero {
    padding: 8px 6px 8px 10px;
    border: 1px solid #d7d7d7;
    border-radius: 6px;
    width: 100%;

    &::placeholder {
      color: #d7d7d7;
    }

    &_icon {
      padding-right: 30px;
    }
  }

  &__icon {
    position: absolute;
    display: flex;
    right: 10px;
    top: 50%;
    transform: translate(0, -50%);
  }
}

textarea {
  resize: none;
  height: 130px;
}
</style>
