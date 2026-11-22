<script setup>
import { computed, ref } from "vue";
import ProductCard from "./components/ProductCard.vue";

const produtos = [
  { id: 1, nome: "Notebook Pro 14\"", categoria: "Notebooks", preco: 5499, precoAntigo: 6199, oferta: true, descricao: "16GB RAM, SSD 512GB NVMe e tela IPS Full HD." },
  { id: 2, nome: "Teclado Mecânico RGB", categoria: "Periféricos", preco: 349, precoAntigo: 429, oferta: true, descricao: "Switches táteis silenciosos e layout ABNT2." },
  { id: 3, nome: "Monitor UltraWide 29\"", categoria: "Monitores", preco: 1390, precoAntigo: null, oferta: false, descricao: "Resolução 2560x1080 com taxa de 100Hz." },
  { id: 4, nome: "Mouse Sem Fio Ergonômico", categoria: "Periféricos", preco: 189, precoAntigo: 239, oferta: true, descricao: "Conexão dual-mode Bluetooth e receptor USB." },
  { id: 5, nome: "SSD Externo 1TB", categoria: "Armazenamento", preco: 520, precoAntigo: null, oferta: false, descricao: "Transferência USB-C de alta velocidade." },
  { id: 6, nome: "Headset Studio ANC", categoria: "Áudio", preco: 699, precoAntigo: 849, oferta: true, descricao: "Cancelamento ativo de ruído e bateria de 40h." },
];

const categorias = ["Todos", "Notebooks", "Periféricos", "Monitores", "Armazenamento", "Áudio"];
const categoriaAtiva = ref("Todos");
const apenasOfertas = ref(false);
const carrinhoIds = ref([]);

const produtosFiltrados = computed(() => {
  return produtos.filter((p) => {
    const matchCategoria = categoriaAtiva.value === "Todos" || p.categoria === categoriaAtiva.value;
    const matchOferta = !apenasOfertas.value || p.oferta;
    return matchCategoria && matchOferta;
  });
});

const totalCarrinho = computed(() => {
  return produtos
    .filter((p) => carrinhoIds.value.includes(p.id))
    .reduce((acc, p) => acc + p.preco, 0);
});

function alternarCarrinho(id) {
  if (carrinhoIds.value.includes(id)) {
    carrinhoIds.value = carrinhoIds.value.filter((item) => item !== id);
  } else {
    carrinhoIds.value.push(id);
  }
}
</script>

<template>
  <div class="container">
    <header class="header">
      <div>
        <span class="badge">Vue 3 • Projeto 02</span>
        <h1>TechStore — Catálogo e Ofertas</h1>
        <p>Equipamentos selecionados com descontos especiais da semana.</p>
      </div>
      <div class="summary">
        <span>Carrinho: <strong>{{ carrinhoIds.length }}</strong> itens</span>
        <strong>Total: R$ {{ totalCarrinho.toFixed(2).replace(".", ",") }}</strong>
      </div>
    </header>

    <section class="toolbar">
      <div class="chips">
        <button
          v-for="cat in categorias"
          :key="cat"
          type="button"
          :class="['chip', { active: categoriaAtiva === cat }]"
          @click="categoriaAtiva = cat"
        >
          {{ cat }}
        </button>
      </div>
      <label class="promo-toggle">
        <input v-model="apenasOfertas" type="checkbox" />
        Mostrar apenas ofertas
      </label>
    </section>

    <main class="grid">
      <ProductCard
        v-for="produto in produtosFiltrados"
        :key="produto.id"
        :produto="produto"
        :no-carrinho="carrinhoIds.includes(produto.id)"
        @alternar-carrinho="alternarCarrinho"
      />
    </main>
  </div>
</template>

<style scoped>
.container {
  max-width: 1080px;
  margin: 0 auto;
  padding: 28px 16px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
  flex-wrap: wrap;
  padding-bottom: 18px;
  border-bottom: 1px solid #334155;
  margin-bottom: 20px;
}

.badge {
  display: inline-block;
  background: rgba(16, 185, 129, 0.16);
  color: #34d399;
  font-size: 0.75rem;
  font-weight: 700;
  padding: 4px 10px;
  border-radius: 999px;
  margin-bottom: 6px;
}

.header p {
  color: #94a3b8;
}

.summary {
  background: #1e293b;
  border: 1px solid #334155;
  padding: 10px 16px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  flex-wrap: wrap;
  margin-bottom: 20px;
}

.chips {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.chip {
  background: #1e293b;
  color: #cbd5e1;
  border: 1px solid #334155;
  padding: 7px 14px;
  border-radius: 999px;
  cursor: pointer;
}

.chip.active {
  background: #10b981;
  color: #06281e;
  border-color: #10b981;
  font-weight: 700;
}

.promo-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.92rem;
  cursor: pointer;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 16px;
}
</style>
