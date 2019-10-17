<template>
	<div>
		<chartjs-doughnut v-bind:labels="barLabels" v-bind:datasets="dataSets" v-bind:option="option"></chartjs-doughnut>

		<!-- Bar graph -->
		<div class="card">
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

		<!-- line graph -->
		<div class="card">
			<div class="card-body">
				<h2 class="card-title">Line</h2>

				<div class="btn-group btn-group-toggle">
					<label
						v-for="(item, index) in btn"
						:key="index"
						:class="{ active: item.value == radio }"
						class="btn btn-success"
					>
						<input v-model="radio" :name="dataLabel" :value="item.value" type="radio" />
						{{ item.label }}
					</label>
				</div>
			</div>

			<div class="card-img-bottom">
				<chartjs-line
					:backgroundcolor="bgColor"
					:beginzero="beginZero"
					:bind="true"
					:bordercolor="borderColor"
					:data="lineData[radio]"
					:datalabel="dataLabel"
					:labels="labels[radio]"
				/>
			</div>
		</div>

		<!-- multiple -->
		<canvas id="mix2" count="2" />
		<chartjs-line
			:backgroundcolor="bgColor"
			:beginzero="beginZero"
			:bind="true"
			:bordercolor="borderColor"
			:data="lineData[radio]"
			:datalabel="dataLabel"
			:labels="labels[radio]"
			target="mix2"
		/>
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
			target="mix2"
		/>
	</div>
</template>

<script>
export default {
	props: ["data"],
	data() {
		return {
			// Pie
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
			beginZero: true,
			barLabels: [],
			types: [
				{
					bgColor: "#de98ab",
					borderColor: "0c0306",
					data: [0, 0, 0, 0, 0, 0, 0, 0],
					dataLabel: "Suburbs"
				}
			],

			// line
			bgColor: "#81894e",
			beginZero: true,
			borderColor: "#81894e",
			btn: [{ label: "state", value: "state" }],
			lineData: {
				state: [0, 0, 0, 0, 0, 0, 0, 0]
			},
			dataLabel: "suburbs",
			labels: {
				state: []
			},
			radio: "state"
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
		this.labels.state = [...new Set(state)];
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
		this.lineData.state = totals;
	}
};
</script>