<script lang="ts">
	// import ApexCharts from 'apexcharts';
    export let data: expense[];
	import { onMount } from 'svelte';
	import type { expense } from '../models';
    const tags = ['Fuel', 'Billing', 'Food', 'Travel', 'Gifts', 'Others', 'Family'];
    let expenses =[0,0,0,0,0,0,0];
    for(let i=0;i<data.length;i++){
        expenses[tags.indexOf(data[i].tag)] += data[i].amount;
    }
	var options = {
		series: expenses,
		chart: {
			width: 380,
			type: 'pie'
		},
		labels: tags,
		responsive: [
			{
				breakpoint: 480,
				options: {
					chart: {
						width: 200
					},
					legend: {
						position: 'bottom'
					}
				}
			}
		]
	};
	onMount(async () => {
		const ApexCharts = (await import('apexcharts')).default;
		const chart = new ApexCharts(document.querySelector('#chart'), options);
		chart.render();
	});
</script>

<svelte:head>
	<script src="https://cdn.jsdelivr.net/npm/apexcharts"></script>
</svelte:head>
<div class="w-full justify-center items-center flex mt-20">
    <div id="chart"></div>
</div>
