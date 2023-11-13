<template>
	<div class="listar">
			<div class="cadastro">
					<label for="descricao">Descrição: </label>
					<input id="descricao" type="text" v-model="descricao" /><br/>
					<label for="dataHoraLimite">Data E Hora Limite: </label>
					<input id="dataHoraLimite" type="datetime-local" v-model="dataHoraLimite" /><br/>
					<label for="peso">Peso: </label>
					<input id="peso" type="number" v-model="peso" /><br/>
					<label for="observacoes">Observações: </label>
					<input id="observacoes" type="text" v-model="observacoes" /><br/>
					<button @click="cadastrar">Cadastrar</button>
			</div>
			<br>
			<table>
					<thead>
							<td>Descrição</td>
							<td>Data E Hora Limite</td>
							<td>Peso</td>
							<td>Observações</td>
					</thead>

					<tr v-for="entrega in entregas" :key="entrega.id">
							<td>{{ entrega.descricao }}</td>
							<td>{{ entrega.dataHoraLimite}}</td> 
							<td>{{ entrega.peso }}</td>
							<td v-if="!entrega.observacoes">Sem Observações</td>
							<td v-else>{{ entrega.observacoes }}</td>
					</tr>
			</table>
			<p>{{ msg }}</p>
			<br/>
			
			<label for="dataHoraLimite2">Data E Hora Limite: </label>
			<input id="dataHoraLimite2" type="datetime-local" v-model="dataHoraLimite2" /><br/>
			<button @click="buscar">Buscar</button><br/>

	</div>
	
</template>


<style>
p{
	color: white;
}
label {
	color: white;
}
table, th, td {
	border:1px solid white;
}

</style>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import axios from 'axios';
const descricao = ref();
const dataHoraLimite = ref();
const peso = ref();
const observacoes = ref();
const dataHoraLimite2 = ref();
const msg = ref();


const entregas = ref();

async function atualizar() {
entregas.value = (await axios.get("https://8080-mineda-springboot3app-3viaonw2jqp.ws-us106.gitpod.io/entrega")).data;
}

async function cadastrar() {
	await axios.post("https://8080-mineda-springboot3app-3viaonw2jqp.ws-us106.gitpod.io/entrega",
	{
		descricao: descricao.value,
		dataHoraLimite: dataHoraLimite.value,
		peso:peso.value,
		observacoes:observacoes.value
	});
	descricao.value = null
	dataHoraLimite.value = null
	peso.value = null
	observacoes.value = null
	atualizar();
}

async function buscar() {
	axios.get(`https://8080-mineda-springboot3app-3viaonw2jqp.ws-us106.gitpod.io/entrega${dataHoraLimite2.value}`)
	.then(function (response) {
			if(response.data < 1){
					msg.value = "Sua busca não foi realizado com sucesso!"
					entregas.value = []
			}
			else{
					msg.value = ""
					entregas.value = response.data
			}
	})
	.catch(function (error) {
			console.log(error);
	});
}

onMounted(() => {
atualizar();
});
</script>