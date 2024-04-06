<script lang="ts">
	import { onMount } from 'svelte';
	import { LineSeries, Chart } from 'svelte-lightweight-charts';
	let data: any;
	let chartData: any = [];
	const formatDate = (date: string) => {
		const l = date.split('-');
		const ns = l[2] + '-' + l[1] + '-' + l[0];
		return ns;
	};
    var unsortedData:any;
	onMount(async () => {
		const response = await fetch('https://api.mfapi.in/mf/118698');
		data = await response.json();
        unsortedData = data.data;
        const sortData =()=>{
            for (let i = 0; i < unsortedData.length; i++) {
                for (let j = 0; j < unsortedData.length - i - 1; j++) {
                    if ( Date.parse(unsortedData[j].date) > Date.parse(unsortedData[j + 1].date)) {
                        let temp = unsortedData[j];
                        unsortedData[j] = unsortedData[j + 1];
                        unsortedData[j + 1] = temp;
                    }
                }
            }
        }
        sortData();


        // date from
		for (let i = 0; i < 10; i++) {
			chartData = [
				...chartData,
				{ time: formatDate(unsortedData.data[i].date), value: parseInt(unsortedData.data[i].nav) }
			];
		}
	});
	const test = [
		{ time: '2019-04-11', value: 80.01 },
		{ time: '2019-04-12', value: 96.63 },
		{ time: '2019-04-13', value: 76.64 },
		{ time: '2019-04-14', value: 81.89 },
		{ time: '2019-04-15', value: 74.43 },
		{ time: '2019-04-16', value: 80.01 },
		{ time: '2019-04-17', value: 96.63 },
		{ time: '2019-04-18', value: 76.64 },
		{ time: '2019-04-19', value: 81.89 },
		{ time: '2024-04-07', value: 100094378.43 }
	];

	const lol = [
		{ time: '2024-04-04', value: 1007.45 },
		{ time: '2024-04-05', value: 1009.46 },
		{ time: '2024-04-07', value: 1008.45 },
		// { time: '2024-04-03', value: 1007.45 },
		// { time: '2024-04-02', value: 1007.45 },
		// { time: '2024-04-01', value: 1006.45 },
		// { time: '2024-04-30', value: 1006.45 },
		// { time: '2024-04-28', value: 1005.45 },
		// { time: '2024-04-27', value: 1005.45 },
		// { time: '2024-04-26', value: 1005.45 }
	];
</script>

<Chart width={600} height={300}>
	<LineSeries data={chartData} />
</Chart>
<p>{JSON.stringify(chartData)}</p>
<p>{JSON.stringify(unsortedData)}</p>
<!-- <p>{JSON.stringify(unsortedData)}</p> -->