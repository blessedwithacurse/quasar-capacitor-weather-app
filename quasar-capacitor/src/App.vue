<template>
  <div id="app" class="container">
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
            { label: 'Light', value: 'light' },
            { label: 'Dark', value: 'dark' },
          ]"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import NavBar from './components/NavBar.vue';
import FilterSelect from './components/Select.vue';
import { useQuasar } from 'quasar';

export default defineComponent({
  name: 'App',
  components: {
    NavBar,
    FilterSelect,
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
  height: 100vh;
  .wrapper {
    .btn-wrapper {
      margin: 16px;
    }
  }
}
</style>
