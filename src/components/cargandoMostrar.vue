<script>
import cargando from '../../js/cargando'

export default {

mounted(){
    this.pantalla = new cargando();
},
methods:{
    mostrarCargando(){
        this.pantalla.contadorAumentando("Cargando...",1)
    }
}

}

</script>

<template>
  <div>
    <h1>ejemplo cargando</h1>
    <select>
      <option value="opcion1">Opción 1</option>
      <option value="opcion2">Opción 2</option>
      <option value="opcion3">Opción 3</option>
    </select>
    <br>
    <input type="text" placeholder="Texto aquí">
    <br>
    <textarea placeholder="Texto aquí"></textarea>
    <br>
    <label><input type="radio" name="opcion" value="opcion1"> Opción 1</label>
    <label><input type="radio" name="opcion" value="opcion2"> Opción 2</label>
    <label><input type="radio" name="opcion" value="opcion3"> Opción 3</label>
  </div>
  <button type="submit" class="btn btn-primary" @click="mostrarCargando()">MostrarPantallaCarga/Guardar</button>
</template>

<style></style>
