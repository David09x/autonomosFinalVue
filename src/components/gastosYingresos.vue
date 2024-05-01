<script>
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
import cargando from '../../js/cargando'
export default {

data(){
    return{
        fechaGuardada1: "",
        fechaGuardada2: "",
        fechaGuardada3: "",
        fechaGuardada4: "",
        beneficioObtenido: null,
        gastosObtenido: null,
        beneficio: "",
        gastos: "",
        beneficio2: "",
        gastos2: ""
    }
},
mounted(){
    this.pantalla = new cargando();
},
methods: {
  
    async ensenyarBeneficios(fecha1, fecha2) {
      const customId = 'custom-id';
      if (fecha1 === "" || fecha2 === "") {
        if (toast.isActive(customId)) {
          toast.update(customId, { type: toast.TYPE.ERROR, render: "Las fechas no pueden estar vacías" });
        } else {
          toast.error("Las fechas no pueden estar vacías", {
            autoClose: 4000,
            pauseOnFocusLoss: false,
            transition: toast.TRANSITIONS.FADE,
            toastId: customId
          });
        }
      } else {
        const fecha_formateada = fecha1.replace(/-/g, '');
        const fecha_formateada2 = fecha2.replace(/-/g, '');
        const partes_fecha = fecha_formateada.split('-');
        const partes_fecha2 = fecha_formateada2.split('-');
        const fechaCalcular = partes_fecha[0];
        const fechaCalcular2 = partes_fecha2[0];

        if (fechaCalcular > fechaCalcular2) {
          if (toast.isActive(customId)) {
            toast.update(customId, { type: toast.TYPE.ERROR, render: "La fecha primera no puede ser mayor que la segunda" });
          } else {
            toast.error("La fecha primera no puede ser mayor que la segunda", {
              autoClose: 4000,
              pauseOnFocusLoss: false,
              transition: toast.TRANSITIONS.FADE,
              toastId: customId
            });
          }
        } else {
          try {
            const json = await this.pantalla.fetchConPromesa("http://localhost:8000/mostrarB/" + fecha1 + "/" + fecha2, "Cargando ...", 2);

            this.beneficio = json.beneficios;
            this.agregarSpan(this.beneficio, "obtenerIngresos", "ingresosObtenidos");

            if (this.beneficio !== "" && this.gastos !== "") {
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.INFO, render: "Total es " + (this.beneficio - this.gastos) });
              } else {
                toast.info("Total es " + (this.beneficio - this.gastos), {
                  position: toast.POSITION.TOP_CENTER,
                  autoClose: false,
                  closeButton: false,
                  hideProgressBar: true,
                  toastId: customId,
                  pauseOnFocusLoss: false,
                  transition: toast.TRANSITIONS.FLIP
                });
              }
            }
          } catch (error) {
            console.error("Error al intentar obtener los beneficios:", error);
          }
        }
      }
    },
    async ensenyarGastos(fecha1, fecha2) {
      if (fecha1 === "" || fecha2 === "") {
        const customId = 'custom-id';
        if (toast.isActive(customId)) {
          toast.update(customId, { type: toast.TYPE.ERROR, render: "Las fechas no pueden estar vacías" });
        } else {
          toast.error("Las fechas no pueden estar vacías", {
            autoClose: 4000,
            pauseOnFocusLoss: false,
            transition: toast.TRANSITIONS.FADE,
            toastId: customId
          });
        }
      } else {
        const fecha_formateada = fecha1.replace(/-/g, '');
        const fecha_formateada2 = fecha2.replace(/-/g, '');
        const partes_fecha = fecha_formateada.split('-');
        const partes_fecha2 = fecha_formateada2.split('-');
        const fechaCalcular = partes_fecha[0];
        const fechaCalcular2 = partes_fecha2[0];

        if (fechaCalcular > fechaCalcular2) {
          const customId = 'custom-id';
          if (toast.isActive(customId)) {
            toast.update(customId, { type: toast.TYPE.ERROR, render: "La fecha primera no puede ser mayor que la segunda" });
          } else {
            toast.error("La fecha primera no puede ser mayor que la segunda", {
              autoClose: 4000,
              pauseOnFocusLoss: false,
              transition: toast.TRANSITIONS.FADE,
              toastId: customId
            });
          }
        } else {
          try {
            const json = await this.pantalla.fetchConPromesa("http://localhost:8000/mostrarG/" + fecha1 + "/" + fecha2, "Cargando ...", 2);

            this.gastos = json.gastos;
            this.agregarSpan(this.gastos, "obtenerGastos", "gastosObtenidos");

            if (this.gastos !== "" && this.beneficio !== "") {
              const total = this.beneficio - this.gastos;
              const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.INFO, render: "Total es " + total });
              } else {
                toast.info("Total es " + total, {
                  position: toast.POSITION.TOP_CENTER,
                  autoClose: false,
                  closeButton: false,
                  hideProgressBar: true,
                  toastId: customId,
                  pauseOnFocusLoss: false,
                  transition: toast.TRANSITIONS.FLIP,
                });
              }
            }
          } catch (error) {
            console.error("Error al intentar obtener los gastos:", error);
          }
        }
      }
    },

    agregarSpan(valor,div,id){
      var spanConprobar =  document.getElementById(id)
      

        if(spanConprobar == null){
        var span =  document.createElement("span");
        span.id =  id
          span.textContent = valor
          var divAnyadir = document.getElementById(div) 
          divAnyadir.appendChild(span)

        }else{
        var span2 = document.getElementById(id)
        span2.textContent =  ""
        span2.textContent = valor
        var divAnyadir = document.getElementById(div) 
          divAnyadir.appendChild(span2)
        }
    },
    calcularGastos() {
      
      var spanConprobar = document.getElementById("calculoTotal");
      if (spanConprobar) {
        // Si el span ya existe, actualizar su contenido
        if (this.beneficio2 !== "" && this.gastos2 !== "") {
          var calculo = (this.beneficio2 - this.gastos2).toFixed(2);
          spanConprobar.textContent = calculo;
        } else {
          const customId = 'custom-id'
          // Verificar si hay un toast activo con el ID "custom-id"
          if (toast.isActive(customId)) {
          // Si hay un toast activo, cerrarlo
            toast.update(customId, { render: "Por favor, completa ambos campos"});
          } else {
            // Si no hay un toast activo, mostrar uno nuevo
            toast.error("Por favor, completa ambos campos", {
              autoClose: 4000,
              pauseOnFocusLoss: false,
              transition: toast.TRANSITIONS.FADE,
              toastId: "custom-id"
            });
          }
        }
      } else {
        // Si el span no existe, crear uno nuevo y agregarlo al contenedor
        if (this.beneficio2 !== "" && this.gastos2 !== "") {
          var span = document.createElement("span");
          span.id = "calculoTotal";
          var divGastosTotales = document.getElementById("gastosIngresoTotal");
          var calculo = (this.beneficio2 - this.gastos2).toFixed(2);
          span.textContent = calculo;
          divGastosTotales.appendChild(span);
        } else {
            const customId = 'custom-id'
        // Verificar si hay un toast activo con el ID "custom-id"
          if (toast.isActive(customId)) {
          // Si hay un toast activo, cerrarlo
            toast.update(customId, { render: "Por favor, completa ambos campos"});
          } else {
            // Si no hay un toast activo, mostrar uno nuevo
            toast.error("Por favor, completa ambos campos", {
              autoClose: 4000,
              pauseOnFocusLoss: false,
              transition: toast.TRANSITIONS.FADE,
              toastId: "custom-id"
            });
          }
        }
      }
    }   
  }
}


</script>

<template>
<div class="container px-5 my-5">
  <h1 class="text-center">Gastos e Ingresos</h1>
  <div class="row">
    <div  class="col-md-6">
      <h2 class="text-center">Ingresos</h2>
      <div class="card border-0 rounded-3 shadow-lg">
        <div class="card-body p-5">
          <div class="form-group">
            <label for="fecha1" class="form-label">Fecha de inicio:</label>
            <input type="date" class="form-control" id="fecha1" name="fecha-poner1" v-model="fechaGuardada1">
          </div>
          <div class="form-group">
            <label for="fecha2" class="form-label">Fecha de fin:</label>
            <input type="date" class="form-control" id="fecha2" name="fecha-poner2" v-model="fechaGuardada2">
          </div>
          <button id="boton" class="btn btn-primary btn-block" @click="ensenyarBeneficios(fechaGuardada1,fechaGuardada2)">Calcular</button>
          <br>
          <div id="obtenerIngresos">

          </div>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <h2 class="text-center">Gastos</h2>
      <div class="card border-0 rounded-3 shadow-lg">
        <div class="card-body p-5">
          <div class="form-group">
            <label for="fecha3" class="form-label">Fecha de inicio:</label>
            <input type="date" class="form-control" id="fecha3" name="fecha-poner1" v-model="fechaGuardada3">
          </div>
          <div class="form-group">
            <label for="fecha4" class="form-label">Fecha de fin:</label>
            <input type="date" class="form-control" id="fecha4" name="fecha-poner2" v-model="fechaGuardada4">
          </div>
          <button id="boton" class="btn btn-primary btn-block" @click="ensenyarGastos(fechaGuardada3,fechaGuardada4)">Calcular</button>
          <br>
          <div id="obtenerGastos" >

          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="text-center mt-4">
    <h2>Diferencia entre ingresos y gastos Manual</h2>
    <div class="card border-0 rounded-3 shadow-lg">
      <div class="card-body p-5">
        <div class="form-group">
          <label for="saasad" class="form-label">Ingresos:</label>
          <input type="number" id="numero1" class="form-control" v-model="beneficio2">
        </div>
        <div class="form-group mt-2">
          <label for="ss" class="form-label">Gastos:</label>
          <input type="number" id="numero1" class="form-control" v-model="gastos2">
        </div>
        <button class="btn btn-primary btn-block mt-3" @click="calcularGastos()">Calcular</button>
        <div id="gastosIngresoTotal" >

        </div>
        <br>
      </div>
    </div>
  </div>
</div>
  </template>
  
  <style scoped>
  .read-the-docs {
    color: #888;
  }


  #obtenerIngresos{
    margin-top: 1rem;
  }
  .form-group{
    margin-top: 10px;
  }
  #label{
    font-size: 2rem
  }
  #boton{
    margin-top: 10px;
    margin-bottom: 5px;
  }

  #numero1{
    width: 40vw;
    margin: 0 auto;
    text-align: center;
    font-size: 2rem;
  }

  #calculoTotal{
    font-size: 2rem;
  }
  </style>
  