<template>
  <div class="wrapper">
    <div class="title" @click="toggleMenu" :style="{ color: settings.getters.getColor() }">
      <div class="selected-option">
        {{showOption}}
      </div>
      <svg :class="{ opened : show_menu }" viewBox="0 0 320 512" width="16">
          <path fill="currentColor" d="M31.3 192h257.3c17.8 0 26.7 21.5 14.1 34.1L174.1 354.8c-7.8
          7.8-20.5 7.8-28.3 0L17.2 226.1C4.6 213.5 13.5 192 31.3 192z"></path>
      </svg>
    </div>
    <div class="sub-menu" v-if="show_menu">
      <div class="menu-item"
          v-for="(item, index) in iterable"
          :key="index"
          @click="updateOption(item)"
          :style="item === showOption ? `color: ${settings.getters.getColor()}`  : `color: inhered`">
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, inject, ref } from 'vue';

export default defineComponent({
  props: {
    iterable: {
      type: Array as () => string[],
      required: true,
    },
    placeholder: {
      type: String,
      required: false,
    },
    selected: {
      type: String,
      required: true,
    },
  },
  setup(props, { emit }) {
    const settings = inject('settings');
    const show_menu = ref(false);
    const selected_option = ref('');

    // @ts-ignore
    const showOption = computed(() => (props.selected.length <= 0 ? props.placeholder
      : selected_option.value));

    function toggleMenu() {
      show_menu.value = !show_menu.value;
    }

    function updateOption(option: string) {
      selected_option.value = option;
      show_menu.value = false;
      emit('update:modelValue', option);
    }

    return {
      show_menu,
      toggleMenu,
      updateOption,
      showOption,
      settings,
    };
  },
});
</script>

<style lang="scss">
@import 'dropdown'
</style>
