<template>
    <div class="login">
      <h1 class="title">Bienvenido!</h1>
      
      
      <h2 class="title">Gestión de Dulces</h2>

      <div class="buttonD">
          <button class="buttonC" @click="logout">Cerrar sesión</button>
      </div>
      <form class="form" @submit.prevent="addDulce">
            <label class="form-label" for="nombre">Nombre del Dulce:</label>
            <input class="form-input" type="text" id="nombre" v-model="nuevoDulce.nombre" required >
            <label class="form-label" for="cantidad">Cantidad:</label>
            <input class="form-input" type="number" id="cantidad" v-model.number="nuevoDulce.cantidad" required>
            <button type="submit" class="button">Agregar Dulce</button>
      </form>
      
      <table class="table">
        <thead>
          <tr>
            <th>Nombre del Dulce</th>
            <th>Cantidad</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(dulce, index) in dulces" :key="index">
            <td>{{ dulce.nombre }}</td>
            <td>{{ dulce.cantidad }}</td>
            <td>
              <button @click="editDulce(index)" class="buttonForm">Editar</button>
              <button @click="deleteDulce(index)" class="buttonForm">Eliminar</button>
            </td>
          </tr>
        </tbody>
      </table>

      
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  
  const user = ref(null);
  const dulces = ref(JSON.parse(localStorage.getItem('dulces')) || []);
  
  const router = useRouter();
  
  const logout = () => {
    user.value = null;
    localStorage.removeItem('user');
    router.push('/');
  }
  
  const nuevoDulce = ref({ nombre: '', cantidad: 0 });
  
  const addDulce = () => {
    dulces.value.push({ ...nuevoDulce.value });
    localStorage.setItem('dulces', JSON.stringify(dulces.value));
    nuevoDulce.value = { nombre: '', cantidad: 0 };
  }
  
  const editDulce = (index: number) => {
    const nuevoNombre = prompt('Ingrese el nuevo nombre del dulce:', dulces.value[index].nombre);
    if (nuevoNombre !== null) {
      const nuevaCantidad = parseInt(prompt('Ingrese la nueva cantidad del dulce:', dulces.value[index].cantidad.toString()) || '0');
      dulces.value[index] = { nombre: nuevoNombre, cantidad: nuevaCantidad };
      localStorage.setItem('dulces', JSON.stringify(dulces.value));
    }
  }
  
  const deleteDulce = (index: number) => {
    dulces.value.splice(index, 1);
    localStorage.setItem('dulces', JSON.stringify(dulces.value));
  }
  </script>

  <style scoped>
  .form {
    margin: 3rem auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 20%;
    min-width: 350px;
    max-width: 100%;
    background-color: #eb71a4;
    border-radius: 5px;
    padding: 40px;
    box-shadow: 0 4px 10px 4px rgba(0, 0, 0, 0.1);
  }
  .table {
    width: 50%;
    border-collapse: collapse;
    margin: 3rem auto;
    background-color: #eb71a4;
    border-radius: 5px;
    box-shadow: 0 4px 10px 4px rgba(0, 0, 0, 0.1);
  }
  .table th,
  .table td {
    padding: 0.5rem;
    text-align: left;
    color: white;
  }
  .form-label{
    color: white;
    font-size: 17px;
  }
  .button{
font-family: Roboto, sans-serif;
font-weight: 0;
font-size: 14px;
color: #fff;
background-color: #d43879;
padding: 10px 30px;
margin: 1rem;
border: solid #e46bff 2px;
border-radius: 50px;
display: flex;
flex-direction: row;
align-items: center;
cursor: pointer;
transition: background-color 0.3s;
}
.button:hover{
    background-color: #d4387963;
}
.buttonForm{
font-family: Roboto, sans-serif;
font-weight: 0;
font-size: 12px;
color: #fff;
background-color: #d43879;
padding: 10px 30px;
border: solid #e46bff 2px;
border-radius: 50px;
align-items: center;
cursor: pointer;
transition: background-color 0.3s;
}
.buttonForm:hover{
    background-color: #d4387963;
}
.form-input{
    padding: 10px 15px;
  background: none;
  border: 1px solid white;
  border-radius: 20px;
  color: white;
}
.buttonD{
display: flex;
justify-content: flex-end;
align-items: flex-start;
}
.buttonC{
font-family: Roboto, sans-serif;
font-weight: 0;
font-size: 14px;
color: #fff;
background-color: #d43879;
padding: 10px 30px;
margin: 1rem;
border: solid #e46bff 2px;
border-radius: 50px;
cursor: pointer;
display: flex;
justify-content: flex-end;
align-items: flex-start;
}
.buttonC:hover{
    background-color: #d4387963;
}

</style>