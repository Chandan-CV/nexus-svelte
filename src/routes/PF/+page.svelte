<script lang="ts">
	import Button from '$lib/components/ui/button/button.svelte';
	import { Item } from '$lib/components/ui/dropdown-menu';
	import Input from '$lib/components/ui/input/input.svelte';
	import Label from '$lib/components/ui/label/label.svelte';
	import { DropdownMenu } from 'bits-ui';
	import { onMount } from 'svelte';
	import Chart from '../../components/Chart.svelte';
	import type { expense } from '../../models';
	import MfChart from '../../components/MFChart.svelte';


	const tags = ['Fuel', 'Billing', 'Food', 'Travel', 'Gifts', 'Others', 'Family'];
	

	let expenses: expense[] = [];
	let tag: string = 'Others';
	let data: any;
	// onMount(async function(){
	//     const response = await fetch("https://api.mfapi.in/mf/118698")
	//     data = await response.json();
	// })
	let income: number = 0;
	let amount: string = '';
	let expenditure: number = 0;

	const addclicked = () => {
		income += parseInt(amount);
	};
	const subtractClicked = () => {
		expenditure += parseInt(amount);
		expenses = [...expenses, { tag: tag as any, amount: parseInt(amount) }]
        console.log(expenses)
    };

   
</script>

<div class="flex flex-col">
	<p>
		Your Income: {income}
	</p>

	<p>
		Your Expenditure: {expenditure}
	</p>

	<p>
		Balance: {income - expenditure}
	</p>
	<p>
		You should invest: {0.2 * income}
	</p>
</div>
<div class="w-full justify-center items-center flex">
	<div class="w-1/3">
		<input bind:value={amount} type="number" />
		<Button on:click={() => addclicked()}>Add</Button>
		<Button on:click={() => subtractClicked()}>Subtract</Button>

		<DropdownMenu.Root>
			<DropdownMenu.Trigger class="border-2 m-5 p-2 rounded">{tag}</DropdownMenu.Trigger>
			<DropdownMenu.Content>
				<DropdownMenu.Group>
					<DropdownMenu.Label class="font-bold">Expenditure spent on</DropdownMenu.Label>
					<DropdownMenu.Separator />
					{#each tags as t}
						<DropdownMenu.Item
							on:click={() => {
								tag = t;
							}}
							class="cursor-pointer">{t}</DropdownMenu.Item
						>
					{/each}
				</DropdownMenu.Group>
			</DropdownMenu.Content>
		</DropdownMenu.Root>
	</div>
</div>

{#key expenses}
<Chart data = {expenses}/>
{/key}

<MfChart/>