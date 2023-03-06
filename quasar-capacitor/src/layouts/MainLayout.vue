<template>
  <div class="container">
    <NavBar />
    <div class="wrapper">
      <FilterSelect />
      <div class="btn-wrapper">
        <q-btn-toggle
          v-model="model"
          push
          glossy
          @click="switchTheme"
          toggle-color="primary"
          :options="[
            { label: $t('light_theme'), value: 'light' },
            { label: $t('dark_theme'), value: 'dark' },
          ]"
        />
      </div>
    </div>
    <LanguageSwitcher />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import NavBar from '../components/NavBar.vue';
import FilterSelect from '../components/Select.vue';
import { useQuasar } from 'quasar';
import LanguageSwitcher from '../components/LanguageSwitcher.vue';

export default defineComponent({
  name: 'MainLayout',
  components: {
    NavBar,
    FilterSelect,
    LanguageSwitcher,
  },
  setup() {
    const $q = useQuasar();
    const model = ref('light');
    const switchTheme = () => {
      if (model.value == 'dark') {
        $q.dark.set(true);
      } else {
        $q.dark.set(false);
      }
    };

    return {
      model,
      switchTheme,
    };
  },
});
</script>

<style lang="scss">
.container {
  .wrapper {
    height: 30%;
    .btn-wrapper {
      margin: 16px;
      display: flex;
    }
  }
}
</style>
