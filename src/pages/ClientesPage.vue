<template>
    <q-form
      @submit="onSubmit"
      @reset="onReset"
    >
    <q-input
        filled
        v-model="form.nome"
        label="Nome "
        lazy-rules
        :rules="[ val => val !== null && val.length !== '' || 'Por favor preencha este campo']"
      />
      <q-input
        filled
        v-model="form.email"
        label="Email "
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Por favor preencha este campo' ]"
      />
      <q-input
        filled
        v-model="form.telefone"
        label="Telefone "
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Por favor preencha este campo' ]"
      />
      <div>
        <q-btn
        label="Enviar"
        color="primary"
        icon="send"
        type="submit" />

        <q-btn
        label="Cancelar"
        type="reset"
        color="white"
        flat class="q-ml-sm"
        style="background-color: red;" />
      </div>
      {{ form}}
    <q-table
    flat bordered
    title="Clientes"
    :rows="rows"
    :columns="columns"
    row-key="name"
    :separator="separator"
    />
    </q-form>
</template>

<script>
import { defineComponent, ref } from 'vue'
import postService from 'src/services/posts'

export default defineComponent({
  name: 'ClientesPage',
  setup () {
    const { post } = postService()
    const columns = [
      {
        name: 'nome',
        required: true,
        label: 'Clientes',
        align: 'left',
        field: row => row.nome,
        format: val => `${val}`,
        sortable: true
      },
      { name: 'email', label: 'Email', field: 'email' },
      { name: 'telefone', label: 'Telefone', field: 'telefone', sortable: true },
      { name: 'protein', label: 'Protein (g)', field: 'protein' }

    ]
    const form = ref({
      nome: '',
      email: '',
      telefone: ''
    })

    const onSubmit = async () => {
      try {
        await post(form.value)
      } catch (error) {
        console.error(error)
      }
    }

    return {
      columns,
      form,
      onSubmit
    }
  }
})

</script>
