<script setup lang="ts">
import {ref,computed,watch} from 'vue';
import {useFetch} from '@vueuse/core';
const props = defineProps<{
    cliente:any;
}>();
const baseURl = 'https://exapi.uttics.com/api/';
const nombre = ref<string>(props.cliente.nombre);
const apellido = ref<string>(props.cliente.apellido);
const email = ref<string>(props.cliente.email);
const telefono = ref<string>(props.cliente.telefono);
const edad = ref<string>(props.cliente.edad);
const direccion = ref<string>(props.cliente.direccion);
const ciudad = ref<string>(props.cliente.ciudad);
const activo = ref<string>(props.cliente.activo);


const nombre2 = ref<string>('');
const apellido2 = ref<string>('');
const email2 = ref<string>('');
const telefono2 = ref<string>('');
const edad2 = ref<string>('');
const direccion2 = ref<string>('');
const ciudad2 = ref<string>('');
const activo2 = ref<string>('');


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
        'fecha_registro':props.cliente.fechaRegistro
    }
});


watch(payload,()=>{
    console.log(payload.value)
})
const emit = defineEmits<{
    (e:'cerrar'):void,
    (e:'enviado'):void
}>()
console.log(props.cliente);

interface form {
    key:string,
    value:string
}
 let errors: Record<string,string> 
async function enviar(){


     nombre2.value ='';
 apellido2.value ='';
 email2.value ='';
 telefono2.value ='';
 edad2.value ='';
 direccion2.value ='';
 ciudad2.value ='';
 activo2.value ='';
   const forms =  Object.entries(payload.value);
  errors = [];
    forms.forEach(([key,value]) => {
        if(value === undefined  || value == ''){
            console.log(true);
        errors.push({key:key,value:'campo vacio'});
        console.log(key);
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
    console.log(nombre2.value);
    if(errors.length == 0){
        await useFetch(`${baseURl}usuarios/${props.cliente.id}`,{
            headers:{
                'Accept':'application/json',
                'Content-type':'application/json'
            }
        }).patch(payload.value);
        emit('enviado');
        console.log('se puede ahcer la peticion');
    }
    
}
</script>


<template>
<div class="modal">
    
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
        <button type="submit" @click="enviar">Guardar</button>
        <button type="button" @click="emit('cerrar')">Cerrar</button>
    </form>
</div> 
</template>


<style scoped>
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
    .form{
        width:100%;
        height:100%;
        display:flex;
        flex-direction:column;
    }
    input{
        width:fit-content;
        height:10px;
    }
</style>