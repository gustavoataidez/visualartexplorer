<template>
  <div id="background">
    <div class="backdrop">
      <h2>Meu Perfil</h2>
      <p>Bem-vindo(a), {{ userEmail }}</p>
      <button class="btn-action" @click="$emit('navigateToCreate')">
        Criar Museu
      </button>

      <div v-if="museums.length > 0" class="museums-list">
        <h3>Museus</h3>
        <div v-for="museum in museums" :key="museum.id">
          <button class="btn-museum" @click="openMuseum(museum.id)">
            {{ museum.title }}
          </button>
        </div>
      </div>

      <button class="btn-secondary" @click="$emit('closeProfile')">
        Fechar
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ProfileModal",
  props: {
    userEmail: {
      type: String,
      required: true,
    },
    userId: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      museums: [], // Armazena todos os museus retornados pela API
    };
  },
  mounted() {
    this.fetchMuseums();
  },
  methods: {
    async fetchMuseums() {
      try {
        // Faz uma chamada à API para buscar todos os museus
        const response = await axios.get("http://localhost:3000/museums");
        this.museums = response.data; // Armazena todos os museus sem filtrar
      } catch (error) {
        console.error("Erro ao buscar os museus:", error);
      }
    },
    openMuseum(museumId) {
      // Redireciona para a página do museu
      this.$router.push(`/museum/edit/${museumId}`);
    },
  },
};
</script>

<style scoped>
#background {
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
}

.backdrop {
  position: absolute;
  max-width: 400px;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 11;
  width: 50%;
  min-width: 300px;
  padding: 30px 20px;
  background: white;
  border-radius: 20px;
  text-align: center;
}

h2 {
  color: var(--vt-c-black);
  margin-bottom: 20px;
  font-weight: bold;
}

h3 {
  color: var(--vt-c-black);
  margin-bottom: 10px;
  font-weight: 600;
}

p {
  color: var(--vt-c-black);
  margin-bottom: 20px;
  font-size: 0.9rem;
}

.museums-list {
  margin-top: 20px;
}

.btn-action {
  background-color: var(--vt-c-orange);
  border: 0px;
  border-radius: 50px;
  font-size: 0.9rem;
  padding: 12px;
  color: white;
  font-weight: bold;
  margin-top: 10px;
  width: 100%;
  text-align: center;
  cursor: pointer;
}

.btn-action:hover {
  opacity: 0.6;
  transition: 0.3s;
}

.btn-museum {
  background-color: var(--vt-c-orange);
  border: 0px;
  border-radius: 20px;
  font-size: 0.9rem;
  padding: 8px;
  color: white;
  font-weight: bold;
  margin: 10px 0;
  width: 100%;
  text-align: center;
  cursor: pointer;
}

.btn-museum:hover {
  opacity: 0.7;
  transition: 0.3s;
}

.btn-secondary {
  background-color: var(--vt-c-black);
  border: 0px;
  border-radius: 50px;
  font-size: 0.9rem;
  padding: 12px;
  color: white;
  font-weight: bold;
  margin-top: 10px;
  width: 100%;
  text-align: center;
  cursor: pointer;
}

.btn-secondary:hover {
  opacity: 0.6;
  transition: 0.3s;
}
</style>