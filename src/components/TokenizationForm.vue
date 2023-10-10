<template>
  <div>
    <!-- Formulario para crear un token -->
    <form @submit.prevent="createToken">
      <input v-model="formData.email" type="text" placeholder="Email" required>
      <input v-model="formData.card_number" type="text" placeholder="Número de tarjeta" required>
      <input v-model="formData.cvv" type="text" placeholder="CVV" required>
      <input v-model="formData.expiration_year" type="text" placeholder="Año de expiración" required>
      <input v-model="formData.expiration_month" type="text" placeholder="Mes de expiración" required>
      <button type="submit">Crear Token</button>
    </form>

    <!-- Mostrar datos de tarjeta por token -->
    <div>
      <input v-model="token" type="text" placeholder="Token">
      <button @click="getCardData">Obtener Datos de Tarjeta</button>
      <div v-if="cardData">
        <p>Email: {{ cardData.email }}</p>
        <p>Número de Tarjeta: {{ cardData.card_number }}</p>
        <!-- Otros campos de datos de tarjeta aquí -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      formData: {
        email: '',
        card_number: '',
        cvv: '',
        expiration_year: '',
        expiration_month: '',
      },
      token: '',
      cardData: null,
    };
  },
  methods: {
    async createToken() {
      try {
        const response = await axios.post(
          'https://api.example.com/createCardToken',
          this.formData
        );
        this.token = response.data.token;
      } catch (error) {
        console.error('Error al crear el token:', error);
      }
    },
    async getCardData() {
      try {
        const response = await axios.get(
          `https://api.example.com/getCardByToken/${token}`
        );
        this.cardData = response.data;
      } catch (error) {
        console.error('Error al obtener datos de tarjeta:', error);
      }
    },
  },
};
</script>

<style scoped>
  /* Estilos para el formulario */
  form {
    display: flex;
    flex-direction: column;
    max-width: 300px;
    margin: 0 auto;
  }

  input {
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
  }

  button {
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
  }

  button:hover {
    background-color: #0056b3;
  }

  /* Estilos para los campos de resultados */
  .results {
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
</style>