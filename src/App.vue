<!-- <script>
export default {

  data() {
    return {
      data: null,
      raca: ''
    }
  },
  methods: {
    async fetchData() {
      //const response = await fetch(`https://dog.ceo/api/breed/${this.raca}/images/random`)
      const response = await fetch(`https://dog.ceo/api/breed/${this.raca}/images`)
      console.log(response.json)
      this.data = await response.json()
    }
  }
};
</script>

<template>
  <div class="initial">
    <img src="./img/Api Dog.svg" alt="">
    <h2>Digite a raça de um cachorro</h2>
    <div>
      <input v-model="raca" type="text" placeholder="Raça">
      <button @click="fetchData"><font-awesome-icon icon="fa-magnifying-glass" style="color: #5a5858;" /></button>
    </div>
  </div>
</template>
<!--<font-awesome-icon icon="fa-magnifying-glass"/>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.initial {
  gap: 15px;
  background-color: #F5F5F5;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.initial div {
  gap: 20px;
  display: flex;
  background-color: #E9E9E9;
  color: #5a5858;
  padding: 12px;
  border-radius: 18px;
  justify-content: space-between;

}

.initial input {
  background-color: transparent;
  border: none;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
}

.initial button {
  background-color: transparent;
  border: none;
}
</style> -->





<script>
export default {
  data() {
    return {
      raca: '',
      fotos: [],
      loading: false
    }
  },
  methods: {
    async fetchData() {
      if (!this.raca) return alert("Por favor, digite uma raça!");

      this.loading = true;
      this.fotos = [];

      try {
        const racaFormatada = this.raca.toLowerCase();
        
        const response = await fetch(`https://dog.ceo/api/breed/${racaFormatada}/images`);
        
        if (!response.ok) {
          throw new Error("Raça não encontrada");
        }

        const data = await response.json();
      
        this.fotos = data.message; 

      } catch (error) {
        alert("Raça não encontrada! Tente nomes em inglês (ex: 'pug', 'labrador').");
      } finally {
        this.loading = false
      }
    }
  }
};
</script>

<template>
  <div class="initial">
    <div class="header-content">
      <img src="./img/Api Dog.svg" alt="Logo" width="80">
      <h2>Galeria de Cachorros</h2>
      
      <div class="search-box">
        <input 
          v-model="raca" 
          type="text" 
          placeholder="Digite a raça (ex: husky)" 
          @keyup.enter="fetchData"
        >
        <button @click="fetchData">
          <font-awesome-icon icon="fa-magnifying-glass" style="color: #5a5858;" />
        </button>
      </div>
      
      <p v-if="loading" class="loading-msg">A buscar todas as fotos... Aguarde.</p>
    </div>

    <div v-if="fotos.length > 0" class="gallery-container">
      
      <div v-for="(foto, index) in fotos" :key="index" class="photo-card">
        <img :src="foto" loading="lazy" alt="Cachorro">
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

.initial {
  background-color: #F5F5F5;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  padding-top: 20px;
  gap: 20px;
}


.header-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  flex-shrink: 0;
}

.search-box {
  gap: 20px;
  display: flex;
  background-color: #E9E9E9;
  color: #5a5858;
  padding: 12px;
  border-radius: 18px;
  justify-content: space-between;
  width: 300px;
}

.initial input {
  background-color: transparent;
  border: none;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
  width: 100%;
}

.initial button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.gallery-container {
  width: 90%;
  max-width: 1200px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 10px;
  overflow-y: auto;
  padding-bottom: 20px; 
  scrollbar-width: thin;
  scrollbar-color: #5a5858 #e0e0e0;
}

.photo-card img {
  width: 100%;
  height: 150px; 
  object-fit: cover;
  border-radius: 8px;
  transition: transform 0.2s;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.photo-card img:hover {
  transform: scale(1.05);
  cursor: pointer;
}

.loading-msg {
  color: #666;
  font-size: 0.9rem;
  font-style: italic;
}
</style>