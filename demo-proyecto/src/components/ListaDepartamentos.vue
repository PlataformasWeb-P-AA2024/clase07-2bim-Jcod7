<template>
  <div>
    <h1>Lista de Departamentos</h1>
    <center>
      <table class="mi-tabla">
        <tr>
          <th>Nombre Propietario</th>
          <th>Costo</th>
          <th>Número de Cuartos</th>
          <th>Edificio</th>
        </tr>
        <tr v-for="departamento in departamentos" :key="departamento.id">
          <td>{{ departamento.nombre_propietario }}</td>
          <td>{{ departamento.costo }}</td>
          <td>{{ departamento.numero_cuartos }}</td>
          <td>{{ departamento.edificioNombre }}</td>
        </tr>
      </table>
    </center>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      departamentos: []
    };
  },
  created() {
    this.fetchDepartamentos();
  },
  methods: {
    async fetchDepartamentos() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/departamentos/');
        const departamentos = response.data.results;
        
        // Agregar nombre del edificio para cada departamento
        for (let departamento of departamentos) {
          try {
            const edificioResponse = await axios.get(departamento.edificio);
            departamento.edificioNombre = edificioResponse.data.nombre;
          } catch (error) {
            console.error('Error obteniendo detalles del edificio:', error);
            departamento.edificioNombre = 'Error obteniendo edificio';
          }
        }

        this.departamentos = departamentos;
      } catch (error) {
        console.error('Error obteniendo departamentos:', error);
      }
    }
  }
};
</script>

<style scoped>
/* agregar ---- */
</style>
