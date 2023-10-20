<template>
      <InputComponent/>
</template>

<script>
import { defineComponent } from 'vue'
import InputComponent from 'src/components/inputs/InputComponent.vue'
export default defineComponent({
  name: 'FormPost',
  components: {
    InputComponent
  }
})
</script>
