<script>
export default {
  data() {
    return {
      raca: '',
      fotos: [],
      carregamento: false
    }
  },
  methods: {
    async fetchData() {
      if (!this.raca) return alert("Por favor digite uma raça!")

      this.carregamento = true
      this.fotos = []

      try {
        const racaFormatada = this.raca.toLowerCase()
        const response = await fetch(`https://dog.ceo/api/breed/${racaFormatada}/images`)
        
        if (!response.ok) {
          throw new Error("Raça não encontrada")
        }

        const data = await response.json()
        this.fotos = data.message

      } catch (error) {
        alert("Raça não encontrada! Tente nomes de cachorro em inglês (ex: 'pug', 'labrador')")
      } finally {
        this.carregamento = false
      }
    }
  }
}
</script>

<template>
  <div class="principal">
    <div class="header">
      <img src="./img/Api Dog.svg" alt="Logo" width="80">
      
      <div class="caixa-pesquisa">
        <input 
          v-model="raca" 
          type="text" 
          placeholder="Digite uma raça em inglês (ex: pug) " 
          @keyup.enter="fetchData"
        >
        <button @click="fetchData">
          <font-awesome-icon icon="fa-magnifying-glass" style="color: #5a5858;" />
        </button>
      </div>
      
      <p v-if="carregamento" class="mensagem-carregamento">Buscando as fotos...</p>
    </div>

    <div v-if="fotos.length > 0" class="container-fotos">
      <div v-for="(foto, index) in fotos" :key="index" class="card-foto">
        <img :src="foto" carregamento="lazy" alt="Cachorro">
      </div>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.principal {
  background-color: #F5F5F5;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  padding-top: 20px;
  gap: 20px;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  flex-shrink: 0; 
}
.header img{
  height: 20vh;
  width: 20vw;
}

.caixa-pesquisa {
  gap: 20px;
  display: flex;
  background-color: #E9E9E9;
  color: #5a5858;
  padding: 12px;
  border-radius: 18px;
  justify-content: space-between;
  width: 300px;
}

.principal input {
  background-color: transparent;
  border: none;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
  width: 100%;
}

.principal button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.container-fotos {
  width: 90%;
  max-width: 1200px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  overflow-y: auto;
  padding-bottom: 20px; 
  scrollbar-width: thin;
  scrollbar-color: #5a5858 #e0e0e0;
}

.card-foto {
  flex: 1 1 150px;
  height: 150px;
  max-width: 300px; 
}

.card-foto img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
  transition: transform 0.2s;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card-foto img:hover {
  transform: scale(1.05);
  cursor: pointer;
}

.mensagem-carregamento {
  color: #666;
  font-size: 0.9rem;
  font-style: italic;
}
</style>