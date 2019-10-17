<template>
	<div>
		<h3>
			filter by suburb
			<input type="search" v-model="search" />
		</h3>
		<br />
		<p>Displaying: {{displayMax}}</p>
		<p>
			Display all?
			<input type="checkbox" @click="displayMax = filteredSearch.length" />
		</p>
		<table>
			<!-- clickable headers for sorting -->
			<th @click="sort('SUBURB')">Suburb</th>
			<th @click="sort('POSTCODE')">Postcode</th>
			<th @click="sort('STATE')">State</th>
			<th @click="sort('COUNTRY')">Country</th>
			<th @click="sort('PO BOX')">Post box</th>
			<th @click="sort('LONG')">Longitude</th>
			<th @click="sort('LAT\r\r')">Latitude</th>
			<tr v-for="object in filteredSearch.slice(0,displayMax)">
				<td>{{object.SUBURB}}</td>
				<td>{{object.POSTCODE}}</td>
				<td>{{object.STATE}}</td>
				<td>{{object.COUNTRY}}</td>
				<td>{{object['PO BOX']}}</td>
				<td>{{object.LONG}}</td>
				<td>{{object['LAT\r\r']}}</td>
			</tr>
		</table>
	</div>
</template>

<script>
export default {
	name: "DisplayTable",
	props: ["data"],
	data() {
		return {
			// Table
			displayMax: 50,
			search: "",
			currentSort: "SUBURB",
			currentSortDir: "ASC"
		};
	},
	methods: {
		sort: function(s) {
			//if s == current sort, reverse
			if (s === this.currentSort) {
				this.currentSortDir =
					this.currentSortDir === "asc" ? "desc" : "asc";
			}
			this.currentSort = s;
		}
	},
	computed: {
		filteredSearch: function() {
			return this.sortedObject.filter(object => {
				// return where data matchs filter
				return object.SUBURB.toLowerCase().match(this.search);
			});
		},
		sortedObject: function() {
			return this.data.sort((a, b) => {
				let modifier = 1;
				if (this.currentSortDir === "desc") modifier = -1;
				if (a[this.currentSort] < b[this.currentSort])
					return -1 * modifier;
				if (a[this.currentSort] > b[this.currentSort])
					return 1 * modifier;
				return 0;
			});
		}
	}
};
</script>
