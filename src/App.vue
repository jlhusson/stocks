<template>
	<div class="row">
		<p><label>Filtre produits dispos</label> <input type="checkbox" v-model="filtre" class="mx-5">
			<label>Trier</label> <select v-model="critereTri">
				<option value="prix">par prix</option>
				<option value="quantite">par stock</option>
			</select>
		</p>
	</div>
	<div class="row g-4">
		<div class="col-md-9 border">
			<h2>Le catalogue</h2>

			<div class="row g-3">
				<div v-for="produit in listeProduitsSelec" class="col-md-4" :key="produit.ident">
					<compProduit :leProduit="produit" @eventAchat="produitAchete" @eventAppro="produitAppro">
					</compProduit>
				</div>
			</div>

		</div>
		<div class="col-md-3 border">
			<h2>Votre panier</h2>
			<p v-if="taillePanier == 0">Votre panier est vide !</p>
			<table v-else class="table table-bordered">
				<caption>Votre panier contient actuellement {{ taillePanier }} produit{{ taillePanier == 1 ? '' : 's' }}
				</caption>
				<tbody>
					<tr v-for="article in panier">
						<td>{{ listeProduits.find((p) => p.ident == article.ident).nom }}</td>
						<td>{{ article.nombre }}</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>

<script setup>

import { ref, computed } from 'vue';
import compProduit from './components/produit.vue';

const filtre = ref(true);
const critereTri = ref('prix');
const listeProduits =ref([]);

const apiUrl = 'https://my.api.mockaroo.com/produitsStock?key=752f86d0';

fetch(apiUrl)
	.then(response => {
		// Vérifier si la requête a réussi
		if (!response.ok) {
			throw new Error('Erreur dans la requête : ' + response.status);
		}
		// Convertir la réponse en JSON
		return response.json();
	})
	.then(data => {
		// Manipuler les données retournées par l'API
		console.log('Données reçues de l\'API :', data);
		listeProduits.value=data ;
	})
	.catch(error => {
		// Gérer les erreurs
		console.error('Erreur :', error);
	});

/*
const listeProduits = ref(
	[{"ident":1,"nom":"Lenovo A328","stock":5,"prix":2346},
{"ident":2,"nom":"Allview Viva 803G","stock":14,"prix":1207},
{"ident":3,"nom":"Samsung Z720","stock":3,"prix":49},
{"ident":4,"nom":"Oppo R9s Plus","stock":13,"prix":1428},
{"ident":5,"nom":"Tecno Pop 2 F","stock":10,"prix":2281},
{"ident":6,"nom":"Siemens S35i","stock":10,"prix":825},
{"ident":7,"nom":"BenQ M220","stock":13,"prix":2197},
{"ident":8,"nom":"HTC U12 life","stock":10,"prix":1151},
{"ident":9,"nom":"Huawei U8687 Cronos","stock":0,"prix":2418},
{"ident":10,"nom":"LG Watch Urbane LTE","stock":5,"prix":1389},
{"ident":11,"nom":"alcatel OT 735","stock":13,"prix":1508},
{"ident":12,"nom":"alcatel OT-710","stock":15,"prix":2025},
{"ident":13,"nom":"BenQ-Siemens E61","stock":1,"prix":1028},
{"ident":14,"nom":"Vodafone Smart Mini","stock":6,"prix":310},
{"ident":15,"nom":"BlackBerry Torch 9810","stock":3,"prix":989},
{"ident":16,"nom":"Philips Diga","stock":15,"prix":434},
{"ident":17,"nom":"Sony Ericsson Windows Phone 7","stock":0,"prix":2033},
{"ident":18,"nom":"Motorola EX210","stock":8,"prix":2498},
{"ident":19,"nom":"ZTE Obsidian","stock":1,"prix":1091},
{"ident":20,"nom":"Panasonic X70","stock":13,"prix":210},
{"ident":21,"nom":"Motorola ROKR Z6","stock":4,"prix":1120},
{"ident":22,"nom":"alcatel Pop 2 (4.5) Dual SIM","stock":15,"prix":1477},
{"ident":23,"nom":"BLU Samba Mini","stock":0,"prix":1505},
{"ident":24,"nom":"Allview X1 Xtreme","stock":11,"prix":2394},
{"ident":25,"nom":"Motorola Moto G5S Plus","stock":0,"prix":1362},
{"ident":26,"nom":"alcatel 1T 10","stock":1,"prix":218},
{"ident":27,"nom":"Samsung U810 Renown","stock":13,"prix":1376},
{"ident":28,"nom":"Sony Xperia XZ1","stock":0,"prix":1804},
{"ident":29,"nom":"Archos Diamond Plus","stock":4,"prix":1026},
{"ident":30,"nom":"Plum Genius","stock":7,"prix":781},
{"ident":31,"nom":"LG Optimus S","stock":10,"prix":362},
{"ident":32,"nom":"Coolpad Torino","stock":11,"prix":1613},
{"ident":33,"nom":"Celkon A89","stock":13,"prix":144},
{"ident":34,"nom":"Samsung Galaxy Pocket plus S5301","stock":5,"prix":1696},
{"ident":35,"nom":"Panasonic Eluga Mark 2","stock":13,"prix":839},
{"ident":36,"nom":"Nokia 3530","stock":3,"prix":1067},
{"ident":37,"nom":"LG Optimus Pad V900","stock":13,"prix":465},
{"ident":38,"nom":"ZTE Style Q","stock":6,"prix":2263},
{"ident":39,"nom":"Parla Zum","stock":13,"prix":2343},
{"ident":40,"nom":"NEC N850","stock":11,"prix":1465},
{"ident":41,"nom":"Samsung X200","stock":13,"prix":171},
{"ident":42,"nom":"Celkon C356","stock":7,"prix":671},
{"ident":43,"nom":"Asus Fonepad 7 FE171CG","stock":4,"prix":1514},
{"ident":44,"nom":"Philips X800","stock":10,"prix":2351},
{"ident":45,"nom":"Palm Centro","stock":14,"prix":2024},
{"ident":46,"nom":"QMobile Noir Z8 Plus","stock":15,"prix":1819},
{"ident":47,"nom":"TCL Plex","stock":5,"prix":126},
{"ident":48,"nom":"LG G4 Pro","stock":15,"prix":1816},
{"ident":49,"nom":"Philips Xenium X806","stock":3,"prix":260},
{"ident":50,"nom":"Lenovo K800","stock":13,"prix":1516}]

);


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
	},
	{
		ident : 9 ,
		nom : 'Enceintes' ,
		prix : 145 ,
		stock : 2
	}
] );*/



const panier = ref([]);


const produitAchete = (idp, message) => {
	console.log('Un produit ' + idp + ' a été acheté : ' + message);
	var laligne = panier.value.find((e) => e.ident == idp);
	if (laligne == undefined) {
		console.log('nouveau prod');
		panier.value.push({ 'ident': idp, 'nombre': 1 });
	}
	else {
		console.log('prod déjà acheté');
		laligne.nombre++;
	};
	listeProduits.value.find((a) => a.ident == idp).stock--;
	console.log(panier.value);
}

const produitAppro = (idp, nb) => {
	console.log('Un produit ' + idp + ' a été réapprovisionné : ' + nb);
	listeProduits.value.find((a) => a.ident == idp).stock += 5;

}

const taillePanier = computed(() => {
	return panier.value.length;
});

const listeProduitsSelec = computed(() => {
	var lp;
	// Filtrage 
	if (filtre.value) {
		lp = listeProduits.value.filter((p) => p.stock > 0);
	}
	else {
		lp = listeProduits.value;
	}

	// tri

	console.log(critereTri.value);
	if (critereTri.value == 'prix') {
		lp = lp.sort((a, b) => a.prix < b.prix)
	}
	else {
		lp = lp.sort((a, b) => a.stock > b.stock)

	}

	return lp;
});

</script>


<style scoped>
input[type=number] {
	width: 4em;
	margin-right: 1em;
}
</style>
