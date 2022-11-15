<template>
  <button
    v-bind="$attrs"
    v-on="$listeners"
    :is="tag"
    :class="[
      'v-button',
      `v-button_${ type }`,
      { 'v-button_round': round },
      { 'v-button_circle': circle }
    ]"
    @mousedown.prevent
  >
    <i v-if="loading">
      <LoadingIcon class="v-button__loading-icon" />
    </i>

    <template v-else>
      <slot v-if="$slots.icon" name="icon" />

      <span v-if="$slots.default">
        <slot />
      </span>

      <slot v-if="$slots.iconRight" name="iconRight" />
    </template>
  </button>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

import LoadingIcon from '@/assets/icons/common/gif-loading.svg';

@Component({
  name: 'VButton',
  components: {
    LoadingIcon,
  },
})
export default class VButton extends Vue {
  @Prop({default: 'button'}) private tag!: string;
  @Prop() private type!: string;
  @Prop({
    default: false,
    type: Boolean,
  }) private loading!: boolean;
  @Prop({
    default: false,
    type: Boolean,
  }) private round!: boolean;
  @Prop({
    default: false,
    type: Boolean,
  }) private circle!: boolean;
}
</script>

<style lang="scss" scoped>
.v-button {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  width: 100%;
  border-radius: 6px;
  padding: 10px;
  color: #52c742;
  font-weight: 500;
  line-height: 115%;
  background-color: #fff;
  box-shadow: 0 5px 15px -1px rgba(87, 87, 87, 0.3);
  transition: .3s;
  border: none;
  cursor: pointer;

  &:hover {
    transform: translate(0, -3px);
  }

  &_success {
    box-shadow: none;
    background-color: #52c742;
    color: #fff;
  }

  &_round {
    border-radius: 20px;
  }

  &_circle {
    border-radius: 50%;
  }

  &_destroy {
    background-color: #fff7f7;
    color: #a40000;
    box-shadow: 0 5px 15px -1px rgba(178, 47, 47, 0.3);
  }

  &__loading-icon {
    width: 70px;
    height: 25px;
  }
}
</style>
