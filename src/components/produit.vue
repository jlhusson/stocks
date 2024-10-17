<template>
   
    <div class="card">
        <div class="card-body" :class="leProduit.stock>0 ? 'bg-success' : 'bg-danger'">
            <h5 class="card-title">Nom : {{leProduit.nom}} ({{leProduit.prix}} €)</h5>
            <h5 class="card-subtitle">Stock : {{leProduit.stock }}</h5>
        </div>
        <div class="card-body">
            <button class="btn btn-outline-primary" :class="leProduit.stock>0 ? '' : 'disabled'" @click="achat(leProduit.ident,'super')">Acheter</button>
            &nbsp;<button class="btn btn-outline-success" @click="approvisionner(leProduit.ident,5)">Réapprovisionner</button>
        </div>
    </div>
</template>
  
<script setup>

    import { defineEmits } from 'vue';

    const props = defineProps({
                leProduit : {
                    type : Object ,
                    required : true
                }
        });

    const emission = defineEmits(['eventAchat','eventAppro']) ;

    function achat(id,msg) {
        emission('eventAchat',id,msg);
    }

    function approvisionner(id,nb) {
        emission('eventAppro',id,nb);
    }

</script>


<style scoped>

    input[type=number] {
        width:4em;
        margin-right: 1em;
    }
</style>
  