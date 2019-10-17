<template>
	<div>
		<button type="button" v-on:click="togglePie">Show Pie Chart</button>
		<button type="button" v-on:click="toggleBar">Show Bar Graph</button>
		<chartjs-doughnut
			v-if="showPie"
			v-bind:labels="barLabels"
			v-bind:datasets="dataSets"
			v-bind:option="option"
		></chartjs-doughnut>

		<div class="card" v-if="showBar">
			<div class="card-body">
				<h2 class="card-title">Bar</h2>
			</div>

			<div class="card-img-bottom">
				<canvas id="fooCanvas" count="1" />

				<chartjs-bar
					v-for="(item, index) in types"
					:key="index"
					:backgroundcolor="item.bgColor"
					:beginzero="beginZero"
					:bind="true"
					:bordercolor="item.borderColor"
					:data="item.data"
					:datalabel="item.dataLabel"
					:labels="barLabels"
					target="fooCanvas"
				/>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: ["data"],
	data() {
		return {
			// Pie
			showPie: false,
			barLabels: [],
			dataSets: [
				{
					data: [0, 0, 0, 0, 0, 0, 0, 0],
					backgroundColor: [
						"Red",
						"Yellow",
						"Purple",
						"Blue",
						"Green",
						"Brown",
						"Pink",
						"Orange"
					]
				}
			],
			option: {
				title: {
					display: true,
					position: "bottom",
					text: "Suburbs by state"
				}
			},

			// Bar
			showBar: false,
			beginZero: true,
			barLabels: [],
			types: [
				{
					bgColor: "#de98ab",
					borderColor: "0c0306",
					data: [0, 0, 0, 0, 0, 0, 0, 0],
					dataLabel: "Suburbs"
				}
			]
		};
	},
	methods: {
		togglePie() {
			this.$forceUpdate();
			this.showPie = !this.showPie;
		},
		toggleBar() {
			this.$forceUpdate();
			this.showBar = !this.showBar;
		}
	},
	created: function() {
		// unique states / labels
		let state = [];
		for (let i = 0; i < this.data.length; i++) {
			state.push(this.data[i].STATE);
		}
		this.barLabels = [...new Set(state)];
		console.log("unique states " + this.barLabels);

		let totals = [0, 0, 0, 0, 0, 0, 0, 0];
		let count = 0;

		// state totals
		for (let i = 0; i < this.data.length; i++) {
			for (let j = 0; j < this.barLabels.length; j++) {
				if (this.barLabels[j] === this.data[i].STATE) {
					totals[j]++;
				}
			}
		}
		console.log("totals " + totals);
		this.types[0].data = totals;
		this.dataSets[0].data = totals;
	}
};
</script>