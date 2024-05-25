<script>
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
import cargando from '../../js/cargando'
export default {
    
data(){
    return {
        nombreCliente: "",
        telefonoCliente:"",
        nombreProveedor: "",
        telefonoProveedor: "",
        estadoP: "",
        estado: "",

    }
},
mounted(){
    this.pantalla = new cargando();
  },

methods: {  
    agregarCliente(nombre, telefono) {
        this.pantalla.fetchConPromesa("http://localhost:8000/cliente/" + nombre + "/" + telefono,"Guardando ...",2)
        .then((json) => {
        this.estado = json;
        const customId = 'custom-id';
        if (this.estado.ok) {
            const successMessage = this.estado.descripcion;
            if (toast.isActive(customId)) {
            toast.update(customId, { type: toast.TYPE.SUCCESS, render: successMessage });
            } else {
            toast.success(successMessage, {
                autoClose: 4000,
                pauseOnFocusLoss: false,
                transition: toast.TRANSITIONS.FADE,
                toastId: customId
            });
            }
            this.nombreCliente = "";
            this.telefonoCliente = "";
            this.nombreProveedor = "";
            this.telefonoProveedor = "";
        } else {
            var comprobanteCorrecto = document.getElementById("telefonoCorrecto");
            if (comprobanteCorrecto === null) {
            document.getElementById("todoCorrecto").innerHTML = "";
            const errorMessage = this.estado.descripcion;
            if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: errorMessage });
            } else {
                toast.error(errorMessage, {
                autoClose: 4000,
                pauseOnFocusLoss: false,
                transition: toast.TRANSITIONS.FADE,
                toastId: customId
                });
            }
            document.getElementById("telefonoIncorrecto").innerHTML = errorMessage;
            } else {
            const successMessage = this.estado.descripcion;
            if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.SUCCESS, render: successMessage });
            } else {
                toast.success(successMessage, {
                autoClose: 4000,
                pauseOnFocusLoss: false,
                transition: toast.TRANSITIONS.FADE,
                toastId: customId
                });
            }
            this.nombreCliente = "";
            this.telefonoCliente = "";
            this.nombreProveedor = "";
            this.telefonoProveedor = "";
            document.getElementById("telefonoCorrecto").innerHTML = "";
            document.getElementById("todoCorrecto").innerHTML = successMessage;
            }
        }
        });
    },

    agregarProveedor(nombre, telefono) {
        this.pantalla.fetchConPromesa("http://localhost:8000/proveedor/" + nombre + "/" + telefono,"Guardando...",2 )
        .then((json) => {
            this.estadoP = json;
            
            if (this.estadoP.ok) {
                const customId = 'custom-id';
                if (toast.isActive(customId)) {
                    toast.update(customId, { type: toast.TYPE.SUCCESS, render: this.estadoP.descripcion });
                } else {
                    toast.success(this.estadoP.descripcion, {
                        autoClose: 4000,
                        pauseOnFocusLoss: false,
                        transition: toast.TRANSITIONS.FADE,
                        toastId: "custom-id"
                    });
                }
                this.nombreCliente = "";
                this.telefonoCliente = "";
                this.nombreProveedor = "";
                this.telefonoProveedor = "";
            } else {
                var comprobanteCorrecto = document.getElementById("telefonoCorrectoP");
                if (comprobanteCorrecto === null) {
                    document.getElementById("todoCorrectoP").innerHTML = "";
                    
                    const customId = 'custom-id';
                    if (toast.isActive(customId)) {
                        toast.update(customId, { type: toast.TYPE.ERROR, render: this.estadoP.descripcion });
                    } else {
                        toast.error(this.estadoP.descripcion, {
                            autoClose: 4000,
                            pauseOnFocusLoss: false,
                            transition: toast.TRANSITIONS.FADE,
                            toastId: "custom-id"
                        });
                    }
                    document.getElementById("telefonoIncorrectoP").innerHTML = this.estadoP.descripcion;
                } else {
                    const customId = 'custom-id';
                    if (toast.isActive(customId)) {
                        toast.update(customId, { type: toast.TYPE.SUCCESS, render: this.estadoP.descripcion });
                    } else {
                        toast.success(this.estadoP.descripcion, {
                            autoClose: 4000,
                            pauseOnFocusLoss: false,
                            transition: toast.TRANSITIONS.FADE,
                            toastId: "custom-id"
                        });
                    }
                    this.nombreCliente = "";
                    this.telefonoCliente = "";
                    this.nombreProveedor = "";
                    this.telefonoProveedor = "";
                    document.getElementById("telefonoCorrectoP").innerHTML = "";
                    document.getElementById("todoCorrectoP").innerHTML = this.estadoP.descripcion;
                }
            }
        });
    },
    comprobacionAgregarCliente(nombre, telefono) {
        const customId = 'custom-id';
        telefono = telefono.trim()
        if (nombre === "" || telefono === "") {
            document.getElementById("telefonoIncorrecto").innerHTML = "";
            document.getElementById("telefonoIncorrecto").innerHTML = "Los campos no pueden estar vacíos";
            if (toast.isActive(customId)) {
            
            toast.update(customId, { type: toast.TYPE.ERROR, render: "Los campos no pueden estar vacíos" });
            } else {
            toast.error("Los campos no pueden estar vacíos", {
                autoClose: 4000,
                pauseOnFocusLoss: false,
                transition: toast.TRANSITIONS.FADE,
                toastId: customId
            });
            }
        } else {
            document.getElementById("telefonoIncorrecto").innerHTML = "";
            if (telefono.length !== 9 || isNaN(telefono)) {
            if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "El teléfono debe tener 9 dígitos y ser numérico" });
            } else {
                toast.error("El teléfono debe tener 9 dígitos y ser numérico", {
                autoClose: 4000,
                pauseOnFocusLoss: false,
                transition: toast.TRANSITIONS.FADE,
                toastId: customId
                });
            }
            document.getElementById("telefonoIncorrecto").innerHTML = "El teléfono debe tener 9 dígitos y ser numérico";
            document.getElementById("nombreC").value = "";
            document.getElementById("telefonoC").value = "";
            this.nombreCliente = "";
            this.telefonoCliente = "";
            } else {
            document.getElementById("telefonoIncorrecto").innerHTML = "";
            document.getElementById("todoCorrecto").innerHTML = "";
            this.agregarCliente(nombre, telefono);
            document.getElementById("todoCorrecto").innerHTML = "Se agregó el cliente";
            setTimeout(() => {
                document.getElementById("todoCorrecto").innerHTML = "";
            }, 1500);
            this.nombreCliente = "";
            this.telefonoCliente = "";
            }
        }
    },
    comprobacionAgregarProveedor(nombre, telefono) {
        const customId = 'custom-id';
        telefono = telefono.trim()
        if (nombre === "" || telefono === "") {
            document.getElementById("telefonoIncorrectoP").innerHTML = "";
            document.getElementById("telefonoIncorrectoP").innerHTML = "Los campos no pueden estar vacíos";
            if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Los campos no pueden estar vacíos" });
            } else {
                toast.error("Los campos no pueden estar vacíos", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: customId
                });
            }
            this.nombreProveedor = "";
            this.telefonoProveedor = "";
        } else {
            document.getElementById("telefonoIncorrectoP").innerHTML = "";
            if (telefono.length !== 9 || isNaN(telefono)) {
                if (toast.isActive(customId)) {
                    toast.update(customId, { type: toast.TYPE.ERROR, render: "El teléfono debe tener 9 dígitos y ser numérico" });
                } else {
                    toast.error("El teléfono debe tener 9 dígitos y ser numérico", {
                        autoClose: 4000,
                        pauseOnFocusLoss: false,
                        transition: toast.TRANSITIONS.FADE,
                        toastId: customId
                    });
                }
                this.nombreProveedor = "";
                this.telefonoProveedor = "";
                document.getElementById("telefonoIncorrectoP").innerHTML = "El teléfono debe tener 9 dígitos y ser numérico";
            } else {
                document.getElementById("telefonoIncorrectoP").innerHTML = "";
                document.getElementById("todoCorrectoP").innerHTML = "";
                this.agregarProveedor(nombre, telefono);
                document.getElementById("todoCorrectoP").innerHTML = "Se agregó el proveedor";
                setTimeout(() => {
                    document.getElementById("todoCorrectoP").innerHTML = "";
                }, 1500); // 1500 milisegundos = 1.5 segundos
                this.nombreProveedor = "";
                this.telefonoProveedor = "";
            }
        }
    },

}

}

</script>

<template>
  
  <div class="container px-5 my-5">
    <div class="row justify-content-center">
        <div class="col-lg-12">
            <div class="row">
                <div class="col-md-6">
                    <div class="card border-0 rounded-3 shadow-lg">
                        <div class="card-body p-5">
                            <div class="text-center">
                                <h1 class="fw-light">Agregar Cliente</h1>
                            </div>
                            <div>
                                <label for="nombreC" class="form-label">Nombre Cliente:</label>
                                <input type="text" id="nombreC" class="form-control" v-model="nombreCliente" required>
                                <label for="telefonoC" class="form-label mt-3">Telefono Cliente:</label>
                                <input type="text" id="telefonoC" class="form-control" v-model="telefonoCliente" required>
                                <button type="submit" class="btn btn-primary mt-3" @click="comprobacionAgregarCliente(nombreCliente,telefonoCliente)">Enviar</button>
                                <div>
                                    <span id="telefonoIncorrecto" style="color:red"></span>
                                    <span id="todoCorrecto" style="color:green"></span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-md-6 mt-5 mt-md-0">
                    <div class="card border-0 rounded-3 shadow-lg">
                        <div class="card-body p-5">
                            <div class="text-center">
                                <h1 class="fw-light">Agregar Proveedores</h1>
                            </div>
                            <div>
                                <label for="nombreP" class="form-label">Nombre Proveedor:</label>
                                <input type="text" id="nombreP" class="form-control" v-model="nombreProveedor" required>
                                <label for="telefonoP" class="form-label mt-3">Telefono Proveedor:</label>
                                <input type="text" id="telefonoP" class="form-control" v-model="telefonoProveedor" required>
                                <button type="submit" class="btn btn-primary mt-3" @click="comprobacionAgregarProveedor(nombreProveedor,telefonoProveedor)">Enviar</button>
                                <div>
                                    <span id="telefonoIncorrectoP" style="color:red"></span>
                                    <span id="todoCorrectoP" style="color:green"></span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

  </template>
  
  <style>
  .read-the-docs {
    color: #888;
  }


  </style>