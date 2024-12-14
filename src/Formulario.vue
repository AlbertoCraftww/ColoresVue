<template>
    <form @submit.prevent="validarInput">
        <input type="text" placeholder="rrr,ggg,bbb" v-model="textoInput">
        <p class="error" v-bind:class="{ visible : error }">fallo</p>
        <input type="submit" value="crear color">
    </form>
</template>

<script setup>
    import { ref } from "vue";

    const textoInput = ref("")
    const error = ref(false)

    const emit = defineEmits(["crear"])

    function validarInput() {

        error.value = false

        if(/^([0-9]{1,3},){3}(0|0\.[1-9]|1)$/.test(textoInput.value)) {
            let valores = textoInput.value.split(",").map( n => Number(n))

            let valido = true

            let i = 0

            while( valido && i < valores.length){
                valido = valores[i] <= 255
                i++
            }

            if (valido){
                let  [r,g,b,a] = valores

                return fetch("https://api-ejemplo-eaxr.onrender.com/colores/nuevo",{
                    method : "POST",
                    body : JSON.stringify({r,g,b,a}),
                    headers : {
                        "Content-type" : "application/json"
                    }
                })
                .then (respuesta => respuesta.json())
                .then(({id}) => {
                    emit("crear",{id,r,g,b,a})
                    return textoInput.value = ""
                })

                
            }
        }

        error.value = true
    }


</script>