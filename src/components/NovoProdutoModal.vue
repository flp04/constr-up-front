<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['salvar', 'fechar'])

const form = reactive({
  nome: '',
  descricao: '',
  marca: '',
  preco: '',
  estoque: ''
})

const salvar = () => {
  emit('salvar', { ...form })

  form.nome = ''
  form.descricao = ''
  form.marca = ''
  form.preco = ''
  form.estoque = ''
}
</script>

<template>
  <div class="modal-overlay">
    <div class="modal-content">
      <h2>Cadastrar Produto</h2>

      <form @submit.prevent="salvar">

        <div class="form-group">
          <label>Nome do Produto:</label>
          <input
            type="text"
            v-model="form.nome"
            required
            placeholder="Ex: Cimento"
          />

          <label>Descrição:</label>
          <input
            type="text"
            v-model="form.descricao"
            required
            placeholder="Ex: Cimento CPII Z 32R"
          />

          <label>Marca:</label>
          <input
            type="text"
            v-model="form.marca"
            required
            placeholder="Ex: Votoran"
          />
        </div>

        <div class="form-group">
          <label>Preço:</label>
          <input
            type="number"
            step="0.01"
            v-model="form.preco"
            required
            placeholder="0.00"
          />

          <label>Estoque:</label>
          <input
            type="number"
            v-model="form.estoque"
            required
            placeholder="0"
          />
        </div>

        <div class="modal-actions">
          <button
            type="button"
            class="btn-cancelar"
            @click="emit('fechar')"
          >
            Cancelar
          </button>

          <button
            type="submit"
            class="btn-salvar"
          >
            Salvar
          </button>
        </div>

      </form>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.5);

  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: white;
  padding: 24px;
  border-radius: 8px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}

.form-group input {
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
}

.btn-salvar {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-cancelar {
  background-color: #6c757d;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}
</style>