<template>
  <q-page padding>
    <q-form
      @submit="onSubmit"
      class="row q-col-gutter-sm"
    >
      <q-input
        outlined
        v-model="form.nome"
        label="Nome"
        lazy-rules
        class="col-lg-6 col-xs-6 input"
        :rules="[ val => val && val.length > 0 || 'Campo obrigatório']"
      />

      <q-input
        outlined
        v-model="form.email"
        label="Email"
        type="email"
        lazy-rules
        class="col-lg-6 col-xs-6 input"
        :rules="[ val => val && val.length > 0 || 'Campo Obrigatório', val => val.includes('@') && val.includes('.com')|| 'Digite seu email corretamente']"
      />
      <q-input
        outlined
        v-model="form.telefone"
        label="Telefone"
        mask="(##) ##### - ####"
        lazy-rules
        class="col-lg-6 col-xs-6 input"
        :rules="[ val => val && val.length > 0 || 'Campo obrigatório']"
      />
        <q-input
          outlined
          v-model="form.date"
          label="Data de Nascimento "
          mask="##/##/####"
          lazy-rules
          class="col-lg-6 col-xs-6 input"
          :rules="[ val => val && val.length > 0 || 'Campo obrigatório',]"
        >
        <template #append>
            <q-icon
          name="event"
          class="cursor-pointer"
          >
              <q-popup-proxy
            cover
            transition-show="scale"
            transition-hide="scale"
          >
            <q-date
            v-model="form.date"
            :mask="mask"
            :rules="[ val => val.length > 0 && val.length < 10]"
            >
            <q-btn
             v-close-popup
             plan
             label="Fechar"
            />
            </q-date>
              </q-popup-proxy>
            </q-icon>

          </template>
        </q-input>

      <div class="col-12 q-gutter-sm">
        <q-btn
          label="Salvar"
          color="primary"
          class="float"
          icon="save"
          type="submit"
        />
        <q-btn
          label="Cancelar"
          color="white"
          class="float"
          text-color="primary"
          :to="{ name: 'home' }"
        />
      </div>

    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import postsService from 'src/services/posts'
import { useQuasar } from 'quasar'
import { useRouter, useRoute } from 'vue-router'
export default defineComponent({
  name: 'FormPost',
  setup () {
    const { post, getById, update } = postsService()
    const $q = useQuasar()
    const router = useRouter()
    const route = useRoute()
    const form = ref({
      nome: '',
      email: '',
      telefone: '',
      date: ''
    })
    const mask = 'DD/MM/YYYY'

    onMounted(async () => {
      if (route.params.id) {
        const response = await getById(route.params.id)
        form.value = response
        getPost(route.params.id)
      }
    })

    const getPost = async (id) => {
      try {
        const response = await getById(id)
        form.value = response
      } catch (error) {
        console.error(error)
      }
    }

    const onSubmit = async () => {
      try {
        if (form.value.id) {
          await update(form.value)
        } else {
          await post(form.value)
        }
        $q.notify({ message: 'Post salvo com sucesso!', icon: 'check', color: 'positive' })
        router.push({ name: 'home' })
      } catch (error) {
        console.error(error)
      }
    }
    return {
      form,
      onSubmit,
      mask

    }
  }
})
</script>
<style lang="scss">
@import '../../node_modules/sass/variables.scss';
div {
  color: $green-5;
}

</style>
