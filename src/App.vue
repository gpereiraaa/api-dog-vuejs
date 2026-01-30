<script>
export default {

  data() {
    return {
      data: null,
      raca: ''
    };
  },
  methods: {
    async fetchData() {
      const response = await fetch(`https://dog.ceo/api/breed/${this.raca}/images/random`);
      console.log(response.json)
      this.data = await response.json();
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
<!--<font-awesome-icon icon="fa-magnifying-glass"/>-->
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
</style>