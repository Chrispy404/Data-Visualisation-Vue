<template>
	<div id="app">
		<input type="file" id="csvdata" @change="loadTextFromFile" />
		<button v-if="data != null" type="button" v-on:click="toggleTable">Show Table</button>
		<!-- {{data}} -->
		<TableDisplay v-if="showTable" v-bind:data="data"></TableDisplay>
	</div>
</template>

<script>
import TableDisplay from "./components/TableDisplay.vue";

export default {
	name: "app",
	components: {
		TableDisplay
	},
	data() {
		return {
			data: null,
			showTable: false
		};
	},
	methods: {
		toggleTable() {
			this.showTable = !this.showTable;
		},
		loadTextFromFile(ev) {
			var files = ev.target.files || ev.dataTransfer.files;
			if (!files.length) return;
			this.createInput(files[0]);
		},
		createInput(file) {
			var reader = new FileReader();
			var vm = this;
			reader.onload = e => {
				vm.data = this.csvJSON(reader.result);
			};
			reader.readAsText(file);
		},
		csvJSON(csv) {
			var lines = csv.split("\n");

			var result = [];

			var headers = lines[0].split(",");

			for (var i = 1; i < lines.length; i++) {
				var obj = {};
				var currentline = lines[i].split(",");

				for (var j = 0; j < headers.length; j++) {
					obj[headers[j]] = currentline[j];
				}
				result.push(obj);
			}
			console.log(result);
			return result; //JavaScript object
			// return JSON.stringify(result); //JSON
			// return JSON.parse(result);
		}
	}
};
</script>

<style>
#app {
	font-family: "Avenir", Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
