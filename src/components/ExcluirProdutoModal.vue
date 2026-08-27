<script setup>
const props = defineProps({
  produto: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['confirmar', 'fechar'])
</script>

<template>
  <div class="modal-overlay">
    <div class="modal-content">

      <h2>Excluir Produto</h2>

      <p>
        Tem certeza que deseja excluir o produto
        <strong>{{ props.produto.nome }}</strong>?
      </p>

      <p class="aviso">
        Essa ação não poderá ser desfeita.
      </p>

      <div class="modal-actions">
        <button
          type="button"
          class="btn-cancelar"
          @click="emit('fechar')"
        >
          Cancelar
        </button>

        <button
          type="button"
          class="btn-excluir"
          @click="emit('confirmar', props.produto.id)"
        >
          Excluir
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

.modal-content h2 {
  margin-top: 0;
  margin-bottom: 20px;
}

.modal-content p {
  margin-bottom: 10px;
  line-height: 1.5;
}

.aviso {
  color: #dc3545;
  font-size: 14px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 25px;
}

.btn-cancelar,
.btn-excluir {
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-cancelar {
  background-color: #6c757d;
  color: white;
}

.btn-excluir {
  background-color: #dc3545;
  color: white;
}

.btn-cancelar:hover {
  background-color: #5a6268;
}

.btn-excluir:hover {
  background-color: #c82333;
}
</style>