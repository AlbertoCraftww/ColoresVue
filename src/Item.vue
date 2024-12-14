<template>
    <li @click="emitir" v-bind:style="{backgroundColor : `rgba(${[r,g,b,a].join(',')})`}">{{ r }},{{ g }},{{ b }},{{ a }}</li>
</template>

<script setup>
const props = defineProps({
    id: Number,
    r: Number,
    g: Number,
    b: Number,
    a: Number
})

const emit = defineEmits(["borrar"]) 

function emitir() {
    fetch("https://api-ejemplo-eaxr.onrender.com/colores/borrar/" + props.id,{
        method : "DELETE"
    })
    .then(respuesta => respuesta.json())
    .then(({error}) => {
        if(!error){
            return emit("borrar", props.id)
        }
        console.log("...error")
    })
}

</script>