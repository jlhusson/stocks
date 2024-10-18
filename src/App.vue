<template>
	<div class="row">
		<p><label>Filtre produits dispos</label> <input type="checkbox" v-model="filtre" class="mx-5">
		<label>Trier</label> <select v-model="critereTri">
			<option value="prix">par prix</option>
			<option value="quantite">par stock</option>
		</select></p>
	</div>
	<div class="row g-4">
		<div class="col-md-8 border">
			<h2>Le catalogue</h2>

			<div class="row">
				<div v-for="produit in listeProduitsSelec" class="col-md-4" :key="produit.ident">
					<compProduit :leProduit="produit" @eventAchat="produitAchete" @eventAppro="produitAppro"></compProduit>
				</div>
			</div>

		</div>
		<div class="col-md-4 border">
		<h2>Votre panier</h2>
		<p v-if="taillePanier==0">Votre panier est vide !</p>
		<table v-else class="table table-bordered">
			<caption>Votre panier contient actuellement {{ taillePanier }} produit{{ taillePanier==1 ? '' : 's' }}</caption>
			<tbody>
				<tr v-for="article in panier">
					<td>{{listeProduits.find((p)=>p.ident==article.ident).nom}}</td>
					<td>{{article.nombre}}</td>
				</tr>
			</tbody>
		</table>
		</div>
  </div>
</template>

<script setup>

	import { ref , computed } from 'vue';
	import compProduit from './components/produit.vue';

	const filtre = ref(true) ;
	const critereTri = ref('prix') ;

	const listeProduits = ref([
		{
			ident : 2 ,
			nom : 'Ecran' ,
			prix : 240 ,
			stock : 3
		} ,
		{
			ident : 7 ,
			nom : 'Clavier' ,
			prix : 30 ,
			stock : 0
		},
		{
			ident : 12 ,
			nom : 'Souris' ,
			prix : 20 ,
			stock : 7
		},
		{
			ident : 8 ,
			nom : 'Stream deck' ,
			prix : 220 ,
			stock : 2
		},
		{
			ident : 1 ,
			nom : 'Téléphone' ,
			prix : 900 ,
			stock : 0
		},
		{
			ident : 4 ,
			nom : 'Tablette' ,
			prix : 600 ,
			stock : 5
		}
	] );

	const panier = ref([]) ;


	const produitAchete = (idp,message) => {
		console.log('Un produit ' + idp + ' a été acheté : ' + message);
    	var laligne = panier.value.find((e)=>e.ident==idp) ;
		if ( laligne == undefined) {
			console.log('nouveau prod') ;
			panier.value.push({'ident':idp,'nombre':1});
		}
		else {
			console.log('prod déjà acheté') ;
			laligne.nombre++ ;
		} ;
		listeProduits.value.find((a)=>a.ident==idp).stock-- ;
		console.log(panier.value) ;
	}

	const produitAppro = (idp,nb) => {
		console.log('Un produit ' + idp + ' a été réapprovisionné : ' + nb);
		listeProduits.value.find((a)=>a.ident==idp).stock+=5 ;

	}

  	const taillePanier = computed(() => {
      return panier.value.length;
    });

	const listeProduitsSelec = computed(() => {
      var lp ;
	  // Filtrage 
	  if (filtre.value ) {
		lp = listeProduits.value.filter((p)=>p.stock>0);
	  }
	  else {
		lp = listeProduits.value ;
	  }

	  // tri

	  console.log (critereTri.value) ;
	  if (critereTri.value=='prix') {
		lp = lp.sort((a,b) => a.prix < b.prix )
	  }
	  else {
		lp = lp.sort((a,b) => a.stock> b.stock )

	  }

	  return lp ;
	});

</script>


<style scoped>

input[type=number] {
	width:4em;
	margin-right: 1em;
}
</style>
