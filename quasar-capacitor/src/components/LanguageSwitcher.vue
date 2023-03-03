<template>
  <div class="select">
    <q-select
      v-model="locale"
      :options="localeOptions"
      :label="$t('switchLanguage')"
      dense
      borderless
      emit-value
      map-options
      options-dense
      @update:model-value="next"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, watchEffect } from 'vue';
import { useI18n } from 'vue-i18n';

export default defineComponent({
  setup() {
    const localeOptions = [
      { key: 'English', label: '🇺🇸 English (US)' },
      { key: 'Русский', label: '🇷🇺 Русский' },
    ];
    const { t, locale } = useI18n();

    function useCycle<T>(list: T[]) {
      const i = ref(0);
      function next() {
        if (i.value >= list.length - 1) {
          i.value = 0;
        } else {
          i.value++;
        }
      }
      const current = computed(() => list[i.value]);
      return {
        current,
        next,
      };
    }
    const { current, next } = useCycle(localeOptions);
    watchEffect(() => {
      locale.value = current.value.key;
    });
    return {
      locale,
      localeOptions,
      current,
      next,
      t,
    };
  },
});
</script>

<style lang="scss">
.select {
  margin: 20px 20px;
  width: 150px;
}
</style>
