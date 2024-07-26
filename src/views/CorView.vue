<script setup>
import { ref, reactive, onMounted } from "vue";
import CoresApi from "@/api/cores";
const coresApi = new CoresApi();

const defaultCor = { id: null, nome: "" };
const cores = ref([]);
const cor = reactive({ ...defaultCor });

onMounted(async () => {
  cores.value = await coresApi.buscarTodasAsCores();
});

function limpar() {
  Object.assign(cor, { ...defaultCor });
}

async function salvar() {
  if (cor.id) {
    await coresApi.atualizarCor(cor);
  } else {
    await coresApi.adicionarCor(cor);
  }
  cores.value = await coresApi.buscarTodasAsCores();
  limpar();
}

function editar(cor_para_editar) {
  Object.assign(cor, cor_para_editar);
}

async function excluir(id) {
  await coresApi.excluirCor(id);
  cores.value = await coresApi.buscarTodasAsCores();
  limpar();
}
</script>

<template>
  <h1>Cor</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="cor.nome" placeholder="Nome" />
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="cor in cores" :key="cor.id">
      <span @click="editar(cor)">
        ({{ cor.id }}) - {{ cor.nome }} -
      </span>
      <button @click="excluir(cor.id)">X</button>
    </li>
  </ul>
</template>

<style>


h1 {
  text-align: center;
  color: #3498db;
  margin: 1rem 0;
}


hr {
  border: 0;
  border-top: 1px solid #ddd;
  margin: 1rem 0;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1rem auto;
  max-width: 400px;
  padding: 1rem;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}


.form input {
  padding: 0.5rem;
  margin-bottom: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  max-width: 300px;
}


.form button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  margin: 0.5rem;
  cursor: pointer;
  background-color: #1d1e1f;
  color: #fff;
  font-size: 1rem;
}

.form button:hover {
  background-color: #2980b9;
}


ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  max-width: 600px;
  margin: auto;
}


li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
  border-bottom: 1px solid #ddd;
  background-color: #fff;
  border-radius: 4px;
  margin-bottom: 0.5rem;
}


li span {
  cursor: pointer;
  color: #3498db;
}


li button {
  padding: 0.25rem 0.5rem;
  border: none;
  border-radius: 4px;
  background-color: #e74c3c;
  color: #fff;
  cursor: pointer;
  font-size: 0.9rem;
}

li button:hover {
  background-color: #c0392b;
}</style> 