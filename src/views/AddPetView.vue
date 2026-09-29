<script setup>
  import { onMounted } from 'vue';
  import { RouterLink, useRoute } from 'vue-router';

  const router = useRoute(); //chamando meu router
  const API_URL = 'http://localhost:3000'; //chamando minha API
  //lista vazia dos meus tutores
  const tutores = ref([]);
  const novoPet = ref({
    nome: '',
    especie: '',
    tutorId: ''
  });

  //buscar todos os tutores salvos na aplicação
  async funtion carregarTutores() {
    const  resposta = await fetch(`${API_URL}/tutores`);

    //converter os dados na minha API que estão em JSON para JS
    tutores.value = await resposta.json();

    console.log(tutores.value);
  }
  //salvar o novo pet no sistema
  async function salvarPet() {
    //enviar os dados do novo pet para a API
    await fetch(`${API_URL}/pets`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(novoPet.value)
    });

    //redirecionar para a página de listagem de pets
    router.push({ name: 'listPets' });


    //redirecionar para a tela de listagem de Pets
    router.push('/pets');
  }
  onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar Pet
    </RouterLink>

    <form class="row"
    @submit.prevent="salvarPet">
    <div class="col-md-6">
      <label
          label for="nome" class="form-label">
      Nome do Pet:
    </label>
    <input
      type="text"
      class="form-control"
      id="nome"
      v-model="novoPet.nome"
      required
      />
      </div>

    <div class="col-md-6">
      <label
      for="form-label"
      class="form-label">Espécie do Pet:
    </label>
    <select
    name="especie"
    class="form-select"
    required
    v-model="novoPet.especie"
    >

    <option value="" disable>Selecione a Espécie</option>
    <option value="Cachorro">Cachorro</option>
    <option value="Gato">Gato</option>
    <option value="Coelho">Coelho</option>
    </select>
    </div>

    <div class="col-md-6">
      <label for="tutor" class="form-label">Tutor:</label>
      <select
        name="tutor"
        id="tutor"
        class="form-select"
        required
        v-model="novoPet.tutorId"
      >
        <option value="">Selecione o Tutor</option>
        <option
          v-for="tutor in tutores"
          :key="tutor.id">
          {{ tutor.nome }}
        </option>
      </select>
    </div>

    <div class="col-md-12 mt-3">
      <button
        type="submit"
        class="btn btn-primary"
      >
        Salvar Pet
      </button>
    </div>
  </form>
  </div>
</template>
