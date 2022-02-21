<script>
import { Bar } from "vue-chartjs";

export default {
	extends: Bar,
	props: {
		chartdata: {
			type: Array,
			required: true,
		},
		chartlabels: {
			type: Array,
			required: true,
		},
		label: {
			type: String,
			required: true,
		},
		title: {
			type: String,
			required: true
		},
		color: {
			type: String,
			required: true
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
				scales: {
					yAxes: [
						{
							ticks: {
								fontColor: 'white',
								beginAtZero: true,
								stepSize: 5,
								max: 30,
							},
							gridLines: {
								display: true,
							},
						},
					],
					xAxes: [
						{
							ticks: {
								fontColor: 'white',
							},
							gridLines: {
								display: false,
							},
						},
					],
				},
				legend: {
            display: false,
        },
				responsive: true,
				maintainAspectRatio: false,
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
						label: this.label,
						pointBackgroundColor: "white",
						backgroundColor: this.color,
					},
				],
			},
			this.options
		);
	},
};
</script>
