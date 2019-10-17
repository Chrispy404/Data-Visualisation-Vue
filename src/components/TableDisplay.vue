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
		<table class="center">
			<!-- clickable headers for sorting -->
			<th @click="sort('SUBURB')">Suburb</th>
			<th @click="sort('POSTCODE')">Postcode</th>
			<th @click="sort('STATE')">State</th>
			<th @click="sort('COUNTRY')">Country</th>
			<th @click="sort('PO BOX')">Post box</th>
			<th @click="sort('LONG')">Longitude</th>
			<th @click="sort('LAT\r\r')">Latitude</th>
			<th>Edit</th>
			<tr v-for="object in filteredSearch.slice(0,displayMax)">
				<td v-if="!edit">{{object.SUBURB}}</td>
				<td v-if="!edit">{{object.POSTCODE}}</td>
				<td v-if="!edit">{{object.STATE}}</td>
				<td v-if="!edit">{{object.COUNTRY}}</td>
				<td v-if="!edit">{{object['PO BOX']}}</td>
				<td v-if="!edit">{{object.LONG}}</td>
				<td v-if="!edit">{{object['LAT\r\r']}}</td>
				<input v-if="edit" v-model="object.SUBURB" placeholder="object.SUBURB" />
				<input v-if="edit" v-model="object.POSTCODE" placeholder="object.POSTCODE" />
				<input v-if="edit" v-model="object.STATE" placeholder="object.STATE" />
				<input v-if="edit" v-model="object.COUNTRY" placeholder="object.COUNTRY" />
				<input v-if="edit" v-model="object['PO BOX']" placeholder="object['PO BOX']" />
				<input v-if="edit" v-model="object.LONG" placeholder="object.LONG" />
				<input v-if="edit" v-model="object['LAT\r\r']" placeholder="object['LAT\r\r']" />
				<td v-on:click="editRow">Edit</td>
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
			edit: false,
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
		},
		editRow() {
			this.edit = !this.edit;
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

<style scoped>
table.center {
	margin-left: auto;
	margin-right: auto;
}
</style>