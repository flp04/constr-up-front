<script setup>
defineProps({
  produto: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['fechar'])
</script>

<template>
  <div class="modal-overlay">
    <div class="modal-content">
      <h2>Visualizar Produto</h2>

      <div class="form-group">
        <label>Nome do Produto:</label>
        <input
          type="text"
          :value="produto.nome"
          disabled
        />

        <label>Descrição:</label>
        <input
          type="text"
          :value="produto.descricao"
          disabled
        />

        <label>Marca:</label>
        <input
          type="text"
          :value="produto.marca"
          disabled
        />
      </div>

      <div class="form-group">
        <label>Preço:</label>
        <input
          type="text"
          :value="`R$ ${Number(produto.preco).toFixed(2)}`"
          disabled
        />

        <label>Estoque:</label>
        <input
          type="text"
          :value="produto.estoque"
          disabled
        />
      </div>

      <div class="modal-actions">
        <button
          type="button"
          class="btn-cancelar"
          @click="emit('fechar')"
        >
          Fechar
        </button>
      </div>
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
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #f5f5f5;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
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