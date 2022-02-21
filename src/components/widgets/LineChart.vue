<script>
import { Line } from "vue-chartjs";

export default {
	extends: Line,
	props: {
		chartdata: {
			type: Array,
			required: true,
		},
		chartlabels: {
			type: Array,
			required: true,
		},
		title: {
			type: String,
			required: true,
		},
	},
	data() {
		return {
			options: {
				title: {
					display: true,
					text: this.title,
					fontSize: 22,
					fontColor: "white"
				},
				legend: {
					display: false,
				},
				scales: {
					yAxes: [
						{
							ticks: {
								fontColor: 'white',
							},
						},
					],
					xAxes: [
						{
							ticks: {
								fontColor: 'white',
							},
						},
					],
				},
				responsive: true,
				maintainAspectRatio: false
			},
		};
	},
	mounted() {
		this.renderChart(
			{
				labels: this.chartlabels,
				datasets: [
					{
						data: this.chartdata,
						pointBackgroundColor: "white",
						backgroundColor: "#BEBDBD",
						color: "white"
					},
				],
			},
			this.options
		);
	},
};
</script>
