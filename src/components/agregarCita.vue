<script>
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
import cargando from '../../js/cargando'
export default {

data(){

    return{servicios:"",telefono:"",hora:"",fecha:"",servicioId:"",telefonoC:"",comprobarCitaAntes:"",
    idClienteBusqueda: "",proveedoresLista:"",proveedorId: "",precioP:"",fechaP:"",
    descripcionP:"",agregarPobjeto:""}
},
mounted(){
  this.pantalla = new cargando();
  this.getServicios()
  this.getProveedores()
},
methods: {

    getProveedores(){
      this.pantalla.fetchConPromesa("http://localhost:8000/mostrarProveedoresLista", "Cargando ...", 1)
      .then((json) => {
          this.proveedoresLista = json.Proveedores;
      })
      .catch((error) => {
          console.error("Error al obtener proveedores:", error);
          if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
          const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
      });
    },

  getServicios(){
    this.pantalla.fetchConPromesa("http://localhost:8000/obtenerServicios", "Cargando ...", 1)
    .then((json)=> {
        this.servicios = json.servicios
    })
    .catch((error) => {
        console.error("Error al obtener servicios:", error);
          if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
          const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
    });
  },

    async guardarGasto(idProveedor, descripcion, precio, fecha) {
        try {
            const response = await this.pantalla.fetchConPromesa("http://localhost:8000/agregarGastos/" + idProveedor + "/" + descripcion + "/" + precio + "/" + fecha, "Guardando...", 2);
            this.agregarPobjeto = response;
            const customId = 'custom-id';
            // Verificar si hay un toast activo con el ID "custom-id"
            if (toast.isActive(customId)) {
                // Si hay un toast activo, cerrarlo
                toast.update(customId, { type: toast.TYPE.SUCCESS, render: "Se agregó con éxito el gasto" });
            } else {
                // Si no hay un toast activo, mostrar uno nuevo
                toast.success("Se agregó con éxito el gasto", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: "custom-id"
                });
            }
        } catch (error) {
            console.error("Error al intentar agregar gasto", error);
            if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
                const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
        }
    },

    async comprobarCliente(telefono) {
        try {
            const response = await this.pantalla.fetchConPromesa("http://localhost:8000/darClienteId/" + telefono, "Guardando...", 2);
            this.telefonoC = response;
        } catch (error) {
            console.error("Error al buscar cliente:", error);
            if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
                const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
        }
    },

    async comprobarCitaPrevia(hora, fecha) {
        try {
            const response = await this.pantalla.fetchConPromesa("http://localhost:8000/buscarCitaPrevia/" + hora + "/" + fecha, "Guardando...", 2);
            this.comprobarCitaAntes = response;
        } catch (error) {
            console.error("Error al intentar buscar cita:", error);
            if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
                const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
        }
    },

    async buscarIdCliente(numero) {
        try {
            const response = await this.pantalla.fetchConPromesa("http://localhost:8000/darClienteId/" + numero, "Guardando...", 2);
            this.idClienteBusqueda = response.idDelCliente[0].id;
           
        } catch (error) {
            console.error("Error al intentar buscar el ID del cliente:", error);
            if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
                const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
        }
    },


    async anyadirCita(idCliente, idServicio, hora, fecha) {
        try {
            const response = await this.pantalla.fetchConPromesa(
                "http://localhost:8000/citas/" + idCliente + "/" + idServicio + "/" + hora + "/" + fecha,
                "Guardando...",2);
            const customId = 'custom-id';
            // Verificar si hay un toast activo con el ID "custom-id"
            if (toast.isActive(customId)) {
                // Si hay un toast activo, cerrarlo
                toast.update(customId, { type: toast.TYPE.SUCCESS, render: "Se agregó con éxito la cita" });
            } else {
                // Si no hay un toast activo, mostrar uno nuevo
                toast.success("Se agregó con éxito la cita", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: "custom-id"
                });
            }
        } catch (error) {
            console.error("Error al intentar añadir cita:", error);
            if(error == "TypeError: NetworkError when attempting to fetch resource." && error instanceof TypeError){
                const customId = 'custom-id';
              if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Se ha perdido la conexión con el servidor por favor revisa que este bien conectado" });
              } else {
                toast.error("Se ha perdido la conexión con el servidor por favor revisa que este bien conectado", {
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
        }
    },

    async agregarCita(numero, hora, fecha) {
      const customId = 'custom-id';
      numero = numero.trim();

      if (numero != "" && hora != "" && fecha != "" && this.servicioId > 0) {
          if (numero.length == 9) {
              await this.comprobarCliente(numero);
              if (this.telefonoC.ok) {
                  await this.comprobarCitaPrevia(hora, fecha);
                  if (this.comprobarCitaAntes.ok) {
                      await this.buscarIdCliente(numero);
                      await this.anyadirCita(this.idClienteBusqueda, this.servicioId, hora, fecha);
                  } else {
                      if (toast.isActive(customId)) {
                          toast.update(customId, { type: toast.TYPE.ERROR, render: this.comprobarCitaAntes.descripcion });
                      } else {
                          toast.error(this.comprobarCitaAntes.descripcion, {
                              autoClose: 4000,
                              pauseOnFocusLoss: false,
                              transition: toast.TRANSITIONS.FADE,
                              toastId: customId
                          });
                      }
                  }
              } else {
                  if (toast.isActive(customId)) {
                      toast.update(customId, { type: toast.TYPE.ERROR, render: this.telefonoC.descripcion });
                  } else {
                      toast.error(this.telefonoC.descripcion, {
                          autoClose: 4000,
                          pauseOnFocusLoss: false,
                          transition: toast.TRANSITIONS.FADE,
                          toastId: customId
                      });
                  }
              }
          } else {
              if (toast.isActive(customId)) {
                  toast.update(customId, { type: toast.TYPE.ERROR, render: "El teléfono debe tener 9 dígitos" });
              } else {
                  toast.error("El teléfono debe tener 9 dígitos", {
                      autoClose: 4000,
                      pauseOnFocusLoss: false,
                      transition: toast.TRANSITIONS.FADE,
                      toastId: customId
                  });
              }
          }
      } else {
          if (toast.isActive(customId)) {
              toast.update(customId, { type: toast.TYPE.ERROR, render: "Debes completar todos los campos y seleccionar un servicio válido" });
          } else {
              toast.error("Debes completar todos los campos y seleccionar un servicio válido", {
                  autoClose: 4000,
                  pauseOnFocusLoss: false,
                  transition: toast.TRANSITIONS.FADE,
                  toastId: customId
              });
          }
      }
    },

    async agregarProveedor(precio,fecha,descripcion){
      if (precio !== "" && fecha !== "" && descripcion !== "" && this.proveedorId > 0) {
          precio = precio.replace(',', '.');     
          if (!isNaN(precio)) {
              precio = parseFloat(precio).toFixed(2);
              await this.guardarGasto(this.proveedorId, descripcion, precio, fecha);
          } else {
              const customId = 'custom-id';

              if (toast.isActive(customId)) {
                  toast.update(customId, { type: toast.TYPE.ERROR, render: "El precio tiene que ser numerico" });
              } else {
                  toast.error("El precio tiene que ser numerico", {
                      autoClose: 4000,
                      pauseOnFocusLoss: false,
                      transition: toast.TRANSITIONS.FADE,
                      toastId: customId
                  });
              }
          }
      } else {
          const customId = 'custom-id';

          if (toast.isActive(customId)) {
              toast.update(customId, { type: toast.TYPE.ERROR, render: "Tienes algún campo vacío" });
          } else {
              toast.error("Tienes algún campo vacío", {
                  autoClose: 4000,
                  pauseOnFocusLoss: false,
                  transition: toast.TRANSITIONS.FADE,
                  toastId: customId
              });
          }
      }

    },

}  
}
</script>

<template>
<div class="container px-5 my-5">
  <div class="row justify-content-center">
    <div class="col-lg-10">
      <div class="row">
        <div class="col-md-6">
          <div class="card border-0 rounded-3 shadow-lg">
            <div class="card-body p-5">
              <div class="text-center">
                <div class="h1 fw-light">Agregar Citas</div>
              </div>
              <form @submit.prevent="submitForm">
                <div class="mb-3">
                  <label for="ntelefono" class="form-label">Telefono:</label>
                  <input type="text" class="form-control" id="ntelefono" v-model="telefono" required>
                </div>
                <div class="mb-3">
                  <label for="tipo" class="form-label text-center">Servicio:</label>
                  <select class="form-select" id="tipo" v-model="servicioId" required>
                    <option v-for="servicio in servicios" :key="servicio.id" :value="servicio.id">{{ servicio.tipo }}</option>
                  </select>
                </div>
                <div class="mb-3">
                  <label for="hora" class="form-label text-center">Hora:</label>
                  <input type="time" class="form-control" id="hora" v-model="hora" required>
                </div>
                <div class="mb-3">
                  <label for="fecha" class="form-label text-center">Fecha:</label>
                  <input type="date" class="form-control" id="fecha" v-model="fecha" required>
                </div>
                <button type="submit" class="btn btn-primary" @click="agregarCita(telefono,hora,fecha)">Guardar</button>
              </form>
            </div>
          </div>
        </div>
        <div class="col-md-6 mt-5 mt-md-0">
          <div class="card border-0 rounded-3 shadow-lg">
            <div class="card-body p-5">
              <div class="text-center">
                <div class="h1 fw-light">Agregar Gasto</div>
              </div>
              <form @submit.prevent="submitForm">
                <div class="mb-3">
                  <label for="tipoProveedor" class="form-label text-center">Proveedor:</label>
                  <select class="form-select" id="tipoProveedor" v-model="proveedorId" required>
                    <option v-for="proveedor in proveedoresLista" :key="proveedor.id" :value="proveedor.id">{{ proveedor.nombre }}</option>
                  </select>
                </div>
                <div class="mb-3">
                  <label for="precio" class="form-label text-center">Precio:</label>
                  <input type="text" class="form-control" id="precio" v-model="precioP" required>
                </div>
                <div class="mb-3">
                  <label for="fechaP" class="form-label text-center">Fecha:</label>
                  <input type="date" class="form-control" id="fechaP" v-model="fechaP" required>
                </div>
                <div class="mb-3">
                  <label for="proveedorD" class="form-label text-center">Descripción:</label>
                  <textarea class="form-control" id="proveedorD" cols="50" rows="2" maxlength="50" placeholder="Descripción aquí" v-model="descripcionP"></textarea>
                </div>
                <button type="submit" class="btn btn-primary" @click="agregarProveedor(precioP,fechaP,descripcionP)">Guardar</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>


</template>

<style scope>

</style>