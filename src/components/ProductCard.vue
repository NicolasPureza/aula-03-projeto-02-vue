<script setup>
defineProps({
  produto: {
    type: Object,
    required: true,
  },
  noCarrinho: {
    type: Boolean,
    default: false,
  },
});

defineEmits(["alternar-carrinho"]);
</script>

<template>
  <article class="card">
    <div class="card-top">
      <span class="cat">{{ produto.categoria }}</span>
      <span v-if="produto.oferta" class="promo">OFERTA</span>
    </div>
    <h3>{{ produto.nome }}</h3>
    <p class="desc">{{ produto.descricao }}</p>
    <div class="price-row">
      <span v-if="produto.precoAntigo" class="old-price">
        R$ {{ produto.precoAntigo.toFixed(2).replace(".", ",") }}
      </span>
      <strong class="price">R$ {{ produto.preco.toFixed(2).replace(".", ",") }}</strong>
    </div>
    <button
      type="button"
      :class="['btn', { 'btn-added': noCarrinho }]"
      @click="$emit('alternar-carrinho', produto.id)"
    >
      {{ noCarrinho ? "Remover do Carrinho" : "Adicionar ao Carrinho" }}
    </button>
  </article>
</template>

<style scoped>
.card {
  background: #1e293b;
  border: 1px solid #334155;
  border-radius: 12px;
  padding: 18px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.card-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cat {
  font-size: 0.78rem;
  color: #38bdf8;
  font-weight: 700;
}

.promo {
  font-size: 0.72rem;
  font-weight: 800;
  background: #16a34a;
  color: #ffffff;
  padding: 2px 8px;
  border-radius: 999px;
}

.desc {
  color: #94a3b8;
  font-size: 0.9rem;
  flex: 1;
}

.price-row {
  display: flex;
  align-items: baseline;
  gap: 8px;
}

.old-price {
  text-decoration: line-through;
  color: #64748b;
  font-size: 0.85rem;
}

.price {
  font-size: 1.15rem;
  color: #f8fafc;
}

.btn {
  border: none;
  background: #0284c7;
  color: #ffffff;
  padding: 10px 12px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

.btn-added {
  background: #dc2626;
}
</style>
