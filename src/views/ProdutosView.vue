<script setup>
import { ref, onMounted } from 'vue'
import api from '@/services/api'

import NovoProdutoModal from '@/components/NovoProdutoModal.vue'
import ProdutoModal from '@/components/ProdutoModal.vue'
import EditarProdutoModal from '@/components/EditarProdutoModal.vue'
import ExcluirProdutoModal from '@/components/ExcluirProdutoModal.vue'

const produtos = ref([])
const produtoSelecionado = ref(null)

const exibirNovoProdutoModal = ref(false)
const exibirProdutoModal = ref(false)
const exibirEditarModal = ref(false)
const exibirExcluirModal = ref(false)


const buscarProdutos = async () => {
  try {
    const response = await api.get('/produtos')
    produtos.value = response.data.data
  } catch (error) {
    console.error(error)
  }
}

const salvarProduto = async (produto) => {
  try {
    const response = await api.post('/produtos', produto)

    produtos.value.push(
      response.data.data
    )

    exibirNovoProdutoModal.value = false

  } catch (error) {
    console.error('Erro ao cadastrar produto:', error)
    alert('Erro ao cadastrar o produto. Verifique os dados.')
  }
}

const atualizarProduto = async (produto) => {
  try {
    const response = await api.put(`/produtos/${produto.id}`, produto)

    const produtoAtualizado = response.data.data

    const index = produtos.value.findIndex(
      item => item.id === produto.id
    )

    if (index !== -1) {
      produtos.value[index] = produtoAtualizado
    }

    exibirEditarModal.value = false

  } catch (error) {
    console.error('Erro ao atualizar produto:', error)
    alert('Erro ao atualizar o produto. Verifique os dados.')
  }
}

const deletarProduto = async (id) => {
  try {
    await api.delete(`/produtos/${id}`)

    produtos.value = produtos.value.filter(
      produto => produto.id !== id
    )

    exibirExcluirModal.value = false

  } catch (error) {
    console.log(error)
    alert('Não foi possível excluir o produto!')
  }
}

const visualizarProduto = (produto) => {
  produtoSelecionado.value = produto
  exibirProdutoModal.value = true
}

const editarProduto = (produto) => {
  produtoSelecionado.value = produto
  exibirEditarModal.value = true
}

const excluirProduto = (produto) => {
  produtoSelecionado.value = produto
  exibirExcluirModal.value = true
}

onMounted(() => {
  buscarProdutos()
})
</script>

<template>
  <div>

    <div class="cabecalho-produtos">
      <h1>Lista de Produtos</h1>

      <button
        class="btn-novo"
        @click="exibirNovoProdutoModal = true"
      >
        + Novo Produto
      </button>
    </div>

    <table class="tabela-produtos">
      <thead>
        <tr>
          <th>Produto</th>
          <th>Preço</th>
          <th>Estoque</th>
          <th class="coluna-acoes">Ações</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="produto in produtos"
          :key="produto.id"
        >
          <td>{{ produto.nome }}</td>

          <td>
            R$ {{ Number(produto.preco).toFixed(2) }}
          </td>

          <td>
            {{ produto.estoque }}
          </td>

          <td class="coluna-acoes">
            <div class="acoes">

              <!-- Visualizar -->
              <button
                class="btn-acao btn-visualizar"
                title="Visualizar produto"
                @click="visualizarProduto(produto)"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7S2 12 2 12z" />
                  <circle cx="12" cy="12" r="3" />
                </svg>
              </button>

              <!-- Editar -->
              <button
                class="btn-acao btn-editar"
                title="Editar produto"
                @click="editarProduto(produto)"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path d="M12 20h9" />
                  <path d="M16.5 3.5a2.1 2.1 0 0 1 3 3L7 19l-4 1 1-4Z" />
                </svg>
              </button>

              <!-- Excluir -->
              <button
                class="btn-acao btn-excluir"
                title="Excluir produto"
                @click="excluirProduto(produto)"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path d="M3 6h18" />
                  <path d="M8 6V4h8v2" />
                  <path d="M19 6l-1 14H6L5 6" />
                  <path d="M10 11v5" />
                  <path d="M14 11v5" />
                </svg>
              </button>

            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modais -->
    <NovoProdutoModal
      v-if="exibirNovoProdutoModal"
      @salvar="salvarProduto"
      @fechar="exibirNovoProdutoModal = false"
    />

    <ProdutoModal
      v-if="exibirProdutoModal"
      :produto="produtoSelecionado"
      @fechar="exibirProdutoModal = false"
    />

    <EditarProdutoModal
      v-if="exibirEditarModal"
      :produto="produtoSelecionado"
      @salvar="atualizarProduto"
      @fechar="exibirEditarModal = false"
    />

    <ExcluirProdutoModal
      v-if="exibirExcluirModal"
      :produto="produtoSelecionado"
      @confirmar="deletarProduto"
      @fechar="exibirExcluirModal = false"
    />

  </div>
</template>

<style scoped>

.cabecalho-produtos {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.cabecalho-produtos h1 {
  margin: 0;
}

.btn-novo {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 9px 16px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
}

.btn-novo:hover {
  background-color: #218838;
}

.tabela-produtos {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.tabela-produtos th,
.tabela-produtos td {
  padding: 12px;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

.tabela-produtos th {
  background-color: #f5f5f5;
  font-weight: bold;
}

.tabela-produtos tbody tr:hover {
  background-color: #fafafa;
}

.coluna-acoes {
  text-align: center !important;
  width: 140px;
}

.acoes {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 6px;
}

.btn-acao {
  width: 32px;
  height: 32px;
  border: none;
  background: transparent;
  cursor: pointer;

  display: flex;
  align-items: center;
  justify-content: center;

  padding: 5px;
  border-radius: 4px;
}

.btn-acao svg {
  width: 18px;
  height: 18px;
}

.btn-visualizar {
  color: #007bff;
}

.btn-editar {
  color: #f0ad4e;
}

.btn-excluir {
  color: #dc3545;
}

.btn-acao:hover {
  background-color: #f0f0f0;
}
</style>