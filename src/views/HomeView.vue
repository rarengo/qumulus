<template>
  <Form-Container />
</template>

<script lang="ts">
import { defineComponent } from 'vue';

// Components
import FormContainer from '../components/FormContainer.vue';

export default defineComponent({
  name: 'HomeView',

  components: {
    FormContainer,
  },
});
</script>
