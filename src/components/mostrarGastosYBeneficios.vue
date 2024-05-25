<script>
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
import cargando from '../../js/cargando'
export default {
    data(){
        return {
            persona:"",
            fechaGuardada1: "",
            fechaGuardada2:"",
            fechaGuardada3: "",
            fechaGuardada4:"",
            gastosM:"",
            beneficiosM: ""
        }
    },
    mounted(){
        this.pantalla = new cargando();
    },
    methods: {
        async getGastos(fecha, fecha2) {
            var  fechaCalcular = fecha.replaceAll("-", '');
            var fechaCalcular2= fecha2.replaceAll("-", '');
            
            if (fecha !== "" && fecha2 !== "") {
                if (fechaCalcular <= fechaCalcular2) {
                try {
                    
                    this.gastosM = await this.pantalla.fetchConPromesa("http://localhost:8000/mostrarGastosFechas/" + fecha + "/" + fecha2, "Cargando ...", 2);
                    if (this.gastosM.ok) {
                    this.ponerTablaGastos(this.gastosM.gastos);
                    } else {
                    var recogerTabla = document.getElementById("tablaGastosBeneficios");
                    recogerTabla.innerHTML = "";
                    const customId = 'custom-id';
                    if (toast.isActive(customId)) {
                        toast.update(customId, { type: toast.TYPE.INFO, render: "No hay gastos entre las fechas seleccionadas" });
                    } else {
                        toast.info("No hay gastos entre las fechas seleccionadas", {
                        autoClose: 4000,
                        pauseOnFocusLoss: false,
                        transition: toast.TRANSITIONS.FADE,
                        toastId: "custom-id"
                        });
                    }
                    }
                } catch (error) {
                    console.error("Error al intentar agregar gasto", error);
                }
                } else {
                var recogerTabla = document.getElementById("tablaGastosBeneficios");
                recogerTabla.innerHTML = "";
                const customId = 'custom-id';
                if (toast.isActive(customId)) {
                    toast.update(customId, { type: toast.TYPE.ERROR, render: "La fecha primera no puede ser mayor que la segunda" });
                } else {
                    toast.error("La fecha primera no puede ser mayor que la segunda", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: "custom-id"
                    });
                }
                }
            } else {
                const customId = 'custom-id';
                if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Las fechas no pueden estar vacías" });
                } else {
                toast.error("Las fechas no pueden estar vacías", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: "custom-id"
                });
                }
            }
        },


        async getBeneficios(fecha, fecha2) {
            var fechaCalcular = fecha.replaceAll("-", '');
            var fechaCalcular2 = fecha2.replaceAll("-", '');

            if (fecha !== "" && fecha2 !== "") {
                if (fechaCalcular <= fechaCalcular2) {
                    try {
                        const json = await this.pantalla.fetchConPromesa("http://localhost:8000/mostrarBeneficiosFechas/" + fecha + "/" + fecha2, "Cargando ...", 2);
                        this.beneficiosM = json;
                        
                        if (this.beneficiosM.ok) {
                        this.ponerTablaBeneficios(this.beneficiosM.beneficios);
                        } else {
                        var recogerTabla = document.getElementById("tablaGastosBeneficios");
                        recogerTabla.innerHTML = "";
                        const customId = 'custom-id';
                        if (toast.isActive(customId)) {
                            toast.update(customId, { type: toast.TYPE.INFO, render: "No hay beneficios entre las fechas seleccionadas" });
                        } else {
                            toast.info("No hay beneficios entre las fechas seleccionadas", {
                            autoClose: 4000,
                            pauseOnFocusLoss: false,
                            transition: toast.TRANSITIONS.FADE,
                            toastId: "custom-id"
                            });
                        }
                        }
                    } catch (error) {
                        console.error("Error al intentar agregar gasto", error);
                    } 
                } else {
                var recogerTabla = document.getElementById("tablaGastosBeneficios");
                recogerTabla.innerHTML = "";
                const customId = 'custom-id';
                if (toast.isActive(customId)) {
                    toast.update(customId, { type: toast.TYPE.ERROR, render: "La fecha primera no puede ser mayor que la segunda" });
                } else {
                    toast.error("La fecha primera no puede ser mayor que la segunda", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: "custom-id"
                    });
                }
                }
            } else {
                const customId = 'custom-id';
                if (toast.isActive(customId)) {
                toast.update(customId, { type: toast.TYPE.ERROR, render: "Las fechas no pueden estar vacías" });
                } else {
                toast.error("Las fechas no pueden estar vacías", {
                    autoClose: 4000,
                    pauseOnFocusLoss: false,
                    transition: toast.TRANSITIONS.FADE,
                    toastId: "custom-id"
                });
                }
            }
        },

        
        ponerTablaGastos(datos) {
            var recogerTabla = document.getElementById("tablaGastosBeneficios");
            recogerTabla.innerHTML = "";

            var tabla = document.createElement("table");
            tabla.className = "table table-striped";
            var thead = document.createElement("thead");
            var encabezadoNombre = document.createElement("tr");

            var nombrePersona = document.createElement("th");
            nombrePersona.setAttribute("scope", "col");
            nombrePersona.textContent = "Nombre";
            var nombreTipo = document.createElement("th");
            nombreTipo.setAttribute("scope", "col");
            nombreTipo.textContent = "descripcion";
            var hora = document.createElement("th");
            hora.setAttribute("scope", "col");
            hora.textContent = "precio";
            var fecha = document.createElement("th");
            fecha.setAttribute("scope", "col");
            fecha.textContent = "fecha";

            encabezadoNombre.appendChild(nombrePersona);
            encabezadoNombre.appendChild(nombreTipo);
            encabezadoNombre.appendChild(hora);
            encabezadoNombre.appendChild(fecha);

            thead.appendChild(encabezadoNombre);
            tabla.appendChild(thead);

            var tbody = document.createElement("tbody");

            for (let i = 0; i < datos.length; i++) {
                var fila = document.createElement("tr");
                var nombre = document.createElement("td");
                nombre.textContent = datos[i]["nombre"];
                var tipo = document.createElement("td");
                tipo.textContent = datos[i]["descripcion"];
                var hora = document.createElement("td");
                hora.textContent = datos[i]["precio"];
                var fecha = document.createElement("td");
                var fechaFormateada = this.formatDate(datos[i]["fecha"]);
                fecha.textContent = fechaFormateada;

                fila.appendChild(nombre);
                fila.appendChild(tipo);
                fila.appendChild(hora);
                fila.appendChild(fecha);

                tbody.appendChild(fila);
            }
            tabla.appendChild(tbody);
            recogerTabla.appendChild(tabla);
        },

        formatDate(fecha) {
            // Formatear la fecha de yyyyMMdd a dd-MM-yyyy
            var año = fecha.substring(0, 4);
            var mes = fecha.substring(4, 6);
            var dia = fecha.substring(6, 8);
            return dia+"-"+mes+"-"+año;
            
        },

        ponerTablaBeneficios(datos) {
            var recogerTabla = document.getElementById("tablaGastosBeneficios");
            recogerTabla.innerHTML = "";

            var tabla = document.createElement("table");
            tabla.className = "table table-striped";
            var thead = document.createElement("thead");
            var encabezadoNombre = document.createElement("tr");

            var nombrePersona = document.createElement("th");
            nombrePersona.setAttribute("scope", "col");
            nombrePersona.textContent = "Nombre";
            var nombreTipo = document.createElement("th");
            nombreTipo.setAttribute("scope", "col");
            nombreTipo.textContent = "Hora";
            var hora = document.createElement("th");
            hora.setAttribute("scope", "col");
            hora.textContent = "Tipo";
            var precio = document.createElement("th");
            precio.setAttribute("scope", "col");
            precio.textContent = "Precio";
            var fecha = document.createElement("th");
            fecha.setAttribute("scope", "col");
            fecha.textContent = "Fecha";

            encabezadoNombre.appendChild(nombrePersona);
            encabezadoNombre.appendChild(nombreTipo);
            encabezadoNombre.appendChild(hora);
            encabezadoNombre.appendChild(precio);
            encabezadoNombre.appendChild(fecha);

            thead.appendChild(encabezadoNombre);
            tabla.appendChild(thead);

            var tbody = document.createElement("tbody");

            for (let i = 0; i < datos.length; i++) {
                var fila = document.createElement("tr");
                var nombre = document.createElement("td");
                nombre.textContent = datos[i]["nombre"];
                var tipo = document.createElement("td");
                tipo.textContent = datos[i]["hora"];
                var hora = document.createElement("td");
                hora.textContent = datos[i]["tipo"];
                var precio = document.createElement("td");
                precio.textContent = datos[i]["precio"];
                var fecha = document.createElement("td");
                var fechaFormateada = this.formatDate(datos[i]["fecha"]);
                fecha.textContent = fechaFormateada;

                fila.appendChild(nombre);
                fila.appendChild(tipo);
                fila.appendChild(hora);
                fila.appendChild(precio);
                fila.appendChild(fecha);

                tbody.appendChild(fila);
            }
            tabla.appendChild(tbody);
            recogerTabla.appendChild(tabla);
        },
    },
}
</script>

<template>
  <div class="row">
    <div class="col-md-6">
        <div id="divGastos" class="form-group">
            <label for="ingreso">Gastos</label>
            <div>
                <input type="date" id="fecha1" class="form-control" name="fecha-poner1" v-model="fechaGuardada1">
                <input type="date" id="fecha2" class="form-control" name="fecha-poner2" v-model="fechaGuardada2">

            </div>
            <button type="submit" class="btn btn-sm btn-primary" @click="getGastos(fechaGuardada1,fechaGuardada2)">Mostrar</button>
        </div>
    </div>
    <div class="col-md-6">
        <div id="divBeneficios" class="form-group">
            <label for="ingreso">Beneficios</label>
            <div>
                <input type="date" id="fecha3" class="form-control" name="fecha-poner3" v-model="fechaGuardada3">
                <input type="date" id="fecha4" class="form-control" name="fecha-poner4" v-model="fechaGuardada4">
            </div>
         
            <button type="submit" class="btn btn-sm btn-primary" @click="getBeneficios(fechaGuardada3,fechaGuardada4)">Mostrar</button>
        </div>
    </div>
</div>
<div id="tablaGastosBeneficios" class="mt-4"> </div>

</template>

<style scoped>
.read-the-docs {
  color: #888;
}

#tablaCitas{
  width: 70vw;
  height: 100vh;
  max-height: 100vh;
  margin: 0 auto;
  padding: 3rem;
  text-align: center; 
}

#divFecha{
  padding: 1rem;
  text-align: center; 
  margin: 0 auto;
}

#fecha1 , #fecha2 , #fecha3, #fecha4{
  width: 150px;
  margin: 0 auto;
  display: inline-block !important;
}

button {
    margin-top: 10px;
    width: 5rem;
    height: 2rem;
    padding: 0 auto

}
</style>