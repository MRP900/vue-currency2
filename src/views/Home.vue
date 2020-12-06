<template>
	<div id="app">
		<Header />
		<Convert v-on:new-convert="convertUSD" v-on:clear-out="clearOut" v-bind:rates="this.cleanData" />
		<table id="results" v-if="conversions.length > 0" class="table table-dark table-striped table-bordered">
			<thead>
				<tr>
					<th>Input</th>
					<th>Output</th>
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
// import * as countries from '../countries.json';
import * as currencies from '../currencies.json';
const apiKey = data.default.key;
export default {
	name: 'Home',
	components: {
		Convert
	},
	methods: {
		convertUSD(money, rateFrom, rateTo) {
			rateFrom = rateFrom.slice(0,3);
			rateTo = rateTo.slice(0,3);
			// console.log(`RateFrom: ${rateFrom}   RateTo: ${rateTo}`);	
			// Convert 'from' to Euro by dividing by exchange rate
			this.converted = money / this.apiResult.rates[rateFrom];
			
			// If not converting to Euro mutliply by target rate
			if (rateTo != 'EUR') {
				this.converted *= this.apiResult.rates[rateTo];	
			}
			// Round final results
			this.converted = this.converted.toFixed(3);
			// Prepare object to hold results
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
			conversions: [],
			
			names: [],
			cleanData: []
		}
	},
	// Get API Data
	created() {
		axios.get('http://data.fixer.io/api/latest?access_key='+apiKey).then(res => {
		
		// Get singular currency name from countryData
		// console.log(res.data.rates);
		this.apiResult = res.data;
		// console.log(this.apiResult);
		// Store abbreviations in array
		this.abb = Object.keys(this.apiResult.rates);
		// console.log(this.abb);
		// Store rates in array
		this.rates = Object.values(this.apiResult.rates);
		// Create new object with id and abbreviation add to array passed to form
		let id = 0;
		this.abb.forEach(element => {
			id++;
			let country = {
			id: id,
			abb: element,
			}
			if (currencies.default[element] !== undefined) {
				country['name'] = currencies.default[element];
			}
			if (res.data.rates[element] !== undefined) {
				country['rate'] =res.data.rates[element];
			}
			this.cleanData.push(country);
		});
		// console.log(this.cleanData);

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
/* table {
	border: 1px solid black;
} */
</style>