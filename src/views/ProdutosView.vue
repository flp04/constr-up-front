<script setup>
import { ref, reactive, onMounted } from 'vue'
import api from '@/services/api'

const produtos = ref([])
// const carregando = ref(true)
// const erro = ref(null)
const exibiModal = ref(false)

const form = reactive({
  nome: '',
  descricao: '',
  marca: '',
  preco: '',
  estoque: ''
})

const buscarProdutos = async() => {
  try {
    // carregando.value = true
    // erro.value = null

    const response = await api.get('/produtos')
    produtos.value = response.data.data
  } catch (error) {
    // erro.value = error.response.data.message
  } finally {
    // carregando.value = false
  }
}

const salvarProduto = async () => {
  try {
    const response = await api.post('/produtos', form)
    
    // Adiciona o produto recém-criado diretamente na lista
    produtos.value.push(response.data.data || response.data)
    
    // Reseta o formulário e fecha o modal
    form.nome = ''
    form.descricao = ''
    form.marca = ''
    form.preco = ''
    form.estoque = ''
    exibiModal.value = false
  } catch (error) {
    console.error('Erro ao cadastrar produto:', error)
    alert('Erro ao cadastrar o produto. Verifique os dados.')
  }
}

const deletarProduto = async (id) => {
  try {
    await api.delete(`/produtos/${id}`)
    produtos.value = produtos.value.filter(produto => produto.id !== id)
  } catch (error) {
    console.log(error)
    alert('Não foi possível excluir o produto!')
  }
}

onMounted(() => {
  buscarProdutos()
  console.log(produtos.value)
})
</script>

<template>
  <div>

    <h1>Lista de Produtos</h1>
    <button class="btn-novo" @click="exibiModal = true">+ Novo Produto</button>
    <ul>
      <li v-for="produto in produtos" :key="produto.id">
        {{ produto.nome }} - R$ {{ produto.preco }} - <button @click="deletarProduto(produto.id)">X</button> 
      </li>
    </ul>

    <div v-if="exibiModal" class="modal-overlay">
      <div class="modal-content">
        <h2>Cadastrar Produto</h2>
        
        <form @submit.prevent="salvarProduto">
          <div class="form-group">
            <label>Nome do Produto:</label>
            <input type="text" v-model="form.nome" required placeholder="Ex: Cimento" />
            <label>Descrição:</label>
            <input type="text" v-model="form.descricao" required placeholder="Ex: Cimento Portland" />
            <label>Marca:</label>
            <input type="text" v-model="form.marca" required placeholder="Ex: Cimento Portland" />
          </div>

          <div class="form-group">
            <label>Preço:</label>
            <input type="number" step="0.01" v-model="form.preco" required placeholder="0.00" />
            <label>Estoque:</label>
            <input type="number" v-model="form.estoque" required placeholder="0" />
          </div>

          <div class="modal-actions">
            <button type="button" class="btn-cancelar" @click="exibiModal = false">Cancelar</button>
            <button type="submit" class="btn-salvar">Salvar</button>
          </div>
        </form>
      </div>
    </div>

  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  font-family: sans-serif;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-novo {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-excluir {
  color: red;
  border: none;
  background: none;
  font-weight: bold;
  cursor: pointer;
  margin-left: 10px;
}

/* Estilos do Modal Backdrop e Card */
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
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
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