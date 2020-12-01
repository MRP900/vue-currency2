<template>
	<div id="app">
		<Header />
		<Convert v-on:new-convert="convertUSD" v-on:clear-out="clearOut" v-bind:rates="this.cleanData" />
		<table id="results" v-if="conversions.length > 0">
			<thead>
				<tr>
					<th>Amount / Converted From</th>
					<th>Amount / Converted to</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="conversion in conversions" v-bind:key="conversion.id">
					<td> {{ conversion.start }} / {{ conversion.from }} </td>
					<td> {{ conversion.end }} / {{ conversion.to }} </td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
import axios from 'axios';
import Convert from '../components/Convert';
import * as data from '../api.json';
import * as countries from '../countries.json';
const apiKey = data.default.key;
export default {
	name: 'Home',
	components: {
		Convert
	},
	methods: {
		convertUSD(money, rateFrom, rateTo) {		
			this.converted = money / this.apiResult.rates[rateFrom];
			
			if (rateTo != 'EUR') {
				this.converted *= this.apiResult.rates[rateTo];	
			}
			this.converted = this.converted.toFixed(2);
			let obj = {}
			obj = {
				id: this.conversions.length,
				start: money,
				end: this.converted,
				from: rateFrom,
				to: rateTo
			}
			
			this.conversions.push(obj);
			console.log(this.conversions);
			
			console.log(this.converted);
		},
		clearOut() {
			this.converted = "";
			this.conversions = [];
			this.conversionId = 0;
		}
		
	},
	data() {
		return {
			apiResult: {},
			rate: "",
			rateFrom: "",
			converted : "",
			// abbreviations
			abb : [],
			rates : [],
			cleanData : [],
			conversions: [],
			countryData: {},
		}
	},
	// Get API Data
	created() {
		axios.get('http://data.fixer.io/api/latest?access_key='+apiKey).then(res => {
		this.countryData = countries;
		this.apiResult = res.data;
		// Store abbreviations in array
		this.abb = Object.keys(this.apiResult.rates);
		// Store rates in array
		this.rates = Object.values(this.apiResult.rates);
		// Create new object with id and abbreviation
		let id = 0;
		this.abb.forEach(element => {
			id++;
			let obj = {
			id: id,
			abb: element,
			}
			this.cleanData.push(obj);
		});
			
			// symbol: symbolShort
		console.log(this.cleanData);
	});
}
}

</script>

<style scoped>

#results {
	margin: 1em auto;
	width: 60%;
	
}
th, td {
	padding: .5em;
}
table {
	border: 1px solid black;
}
</style>