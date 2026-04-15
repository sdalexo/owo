<script setup lang="ts">
import {useFetch} from '@vueuse/core';
import {watch,ref,computed}from 'vue';
import Campo from '@/components/Campo.vue';
const baseURl = 'https://exapi.uttics.com/api/';
const cliente = ref<any>(null);
const showmodal2 = ref<boolean>(false);
const showEliminar = ref<boolean>(false);
const {data,isFetching} = useFetch(`${baseURl}usuarios`).get().json();
const añadir = ref<boolean>(false);

async function refetch(){
    isFetching.value = true;
   const {data:data2}= await useFetch(`${baseURl}usuarios`).get().json();
   data.value = data2.value;
     isFetching.value = false;
}

watch(data,()=>{
    console.log(data.value);
})

function showmodal(selected){
    showmodal2.value = true;
    cliente.value = selected;
}
async function eliminar(selected){
 cliente.value = selected;
 console.log()
    showEliminar.value = true;
    
}
async function eliminarsi(){
    console.log(cliente);
    const {data,isFetching} = await useFetch(`${baseURl}usuarios/${cliente.value.id}`).delete().json();
    showEliminar.value = false;
    refetch();
}


//añadir

const nombre = ref<string>('');
const apellido = ref<string>('');
const email = ref<string>('');
const telefono = ref<string>('');
const edad = ref<string>('');
const direccion = ref<string>('');
const ciudad = ref<string>('');
const activo = ref<boolean>(true);

const payload = computed(()=>{
    return {
        'nombre':nombre.value,
        'apellido':apellido.value,
        'email':email.value,
        'telefono':telefono.value,
        'edad':edad.value,
        'direccion':direccion.value,
        'ciudad':ciudad.value,
        'activo':activo.value,
        'fecha_registro':'2006-04-15'
    }
});


const nombre2 = ref<string>('');
const apellido2 = ref<string>('');
const email2 = ref<string>('');
const telefono2 = ref<string>('');
const edad2 = ref<string>('');
const direccion2 = ref<string>('');
const ciudad2 = ref<string>('');
const activo2 = ref<string>('');
const number = ref<number>(0);

watch(data,()=>{
    number.value = 0;
    data.value?.data?.forEach(element => {
       number.value++;
    });
});
 let errors: Record<string,string> 
async function enviarñaadir(){


     nombre2.value ='';
 apellido2.value ='';
 email2.value ='';
 telefono2.value ='';
 edad2.value ='';
 direccion2.value ='';
 ciudad2.value ='';
 activo2.value ='';
   errors = [];
   const forms =  Object.entries(payload.value);
    forms.forEach(([key,value]) => {
        if(value === undefined  || value == ''){
              errors.push({key:key,value:'campo vacio'});
        switch (key) {
            case 'nombre':
                nombre2.value = 'campo vacio'
                break;
            case 'apellido':
                apellido2.value = 'campo vacio'
                break; 
                case 'email':
                email2.value = 'campo vacio'
                break;
            case 'telefono':
                telefono2.value = 'campo vacio'
                break; 
                case 'edad':
                edad2.value = 'campo vacio'
                break;
            case 'direccion':
                direccion2.value = 'campo vacio'
                break; 
                case 'ciudad':
                ciudad2.value = 'campo vacio'
                break;
            case 'activo':
                activo2.value = 'campo vacio'
                break;    
        
            default:
                break;
        }
        }
    
    });
    console.log(errors);
    if(errors.length == 0){
        console.log('entro al fetch')
        await useFetch(`${baseURl}usuarios`,{
            headers:{
                'Accept':'application/json',
                'Content-type':'application/json'
            }
        }).post(payload.value);
    añadir.value = false;
    refetch();
        console.log('se puede ahcer la peticion');
    }
    
}

</script>


<template>
<p>Cantidad de usuarios : {{number}}</p>
<div class="modal" v-if="showEliminar">
    <p>Desea eliminar</p>
    <button type="button" @click="eliminarsi(selected)">Si</button>
    <button type="button">Ño</button>
</div>
    <Campo v-if="showmodal2" @cerrar="showmodal2=false" @enviado="{refetch();showmodal2=false;}":cliente="cliente"/>
   <h3>ESto es un dashboard muy interactivo xd</h3>
    <div v-if="isFetching">
        cargando . .
    </div>

   <div v-for="clientes in data?.data">
        <div style="display:flex;gap:10px; align-items:center;">
          <!--  <p>{{clientes.id}}</p>-->
        <p>{{clientes.nombre}}</p>
        <p>{{clientes.apellido}}</p>
        <p>{{clientes.email}}</p>
       <!--- <p>{{clientes.telefono}}</p>
        <p>{{clientes.edad}}</p>
        <p>{{clientes.direccion}}</p>
        <p>{{clientes.activo}}</p>
        <p>{{clientes.fechaRegistro}}</p>-->
        <button type="button" style="height:fit-content" @click="showmodal(clientes)">Editar</button>
         <button type="button" style="height:fit-content" @click="eliminar(clientes)">Eliminar</button>
        </div>
   </div>

   <button type="button" @click="añadir=true">Añadir</button>



   <div class="modal" v-if="añadir">
    
    <h2>Formulario muy chido</h2>
    <form class="form" @submit.prevent>
        
        <label for="">Nombre</label>
        <span>{{nombre2}}</span>
        <input type="text" name="" v-model="nombre">
        <label for="">apellido</label>
        <span>{{apellido2}}</span>
        <input type="text" name="" v-model="apellido">
        <label for="">email</label>
        <span>{{email2}}</span>
        <input type="text" name="" v-model="email">
        <label for="">telefono</label>
        <span>{{telefono2}}</span>
        <input type="text" name="" v-model="telefono">
        <label for="">edad</label>
        <span>{{edad2}}</span>
        <input type="number" name="" v-model="edad">
        <label for="">direccion</label>
        <span>{{direccion2}}</span>
        <input type="text" name="" v-model="direccion">
        <label for="">ciudad</label>
        <span>{{ciudad2}}</span>
        <input type="text" name="" v-model="ciudad">
        <label for="">activo</label>
        <span>{{activo2}}</span>
        <input type="text" name="" v-model="activo">
        <button type="submit" @click="enviarñaadir">Guardar</button>
        <button type="button" @click="añadir = false;">Cerrar</button>
    </form>
</div> 
</template>


<style >
    .modal{
        background-color:gray;
        width:100%;
        height:100vh;
        position:absolute;
        top:0px;
        left:0px;
        z-index:1000;
        trasform:translate(-50%,-50%)
    }
</style>