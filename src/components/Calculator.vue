<script setup>
	import { ref, computed } from 'vue';

	const actions = ref([
		{
			type: 'Add',
			symbol: '+',
		},
		{
			type: 'Minus',
			symbol: '-',
		},
		{
			type: 'Equal',
			symbol: '=',
		},
		{
			type: 'Clear',
			symbol: 'C',
		},
		// {
		// 	type: 'percentage',
		// 	symbol: '%',
		// },
	]);
	const result = ref('');
	const previous = ref(null);
	const operator = ref(null);
	const operatorClicked = ref(false);

	const handleDigits = (num) => {
		if (operatorClicked.value) {
			result.value = '';
			operatorClicked.value = false;
		}
		result.value = `${result.value}${num}`;
	};
	const setPrevious = () => {
		previous.value = result.value;
		operatorClicked.value = true;
	};
	const handleAction = (action) => {
		if (action.type === 'Clear') {
			result.value = '';
		}
		if (action.type === 'percentage') {
			result.value = `${parseFloat(result.value) / 100}`;
		}
		if (action.type === 'Add') {
			operator.value = (a, b) => a + b;
			setPrevious();
		}
		if (action.type === 'Minus') {
			operator.value = (a, b) => a - b;
			setPrevious();
		}
		if (action.type === 'Multiply') {
			operator.value = (a, b) => a * b;
			setPrevious();
		}
		if (action.type === 'Divide') {
			operator.value = (a, b) => a / b;
			setPrevious();
		}
		if (action.type === 'Equal') {
			result.value = `${operator.value(
				parseFloat(previous.value),
				parseFloat(result.value)
			)}`;
		}
	};
</script>

<template>
	<h1 class="title">Calcie</h1>

	<main class="cal">
		<div class="numpad">
			<input readonly v-model="result" class="result" />
			<div class="row">
				<h1 @click="handleDigits('1')">1</h1>
				<h1 @click="handleDigits('2')">2</h1>
				<h1 @click="handleDigits('3')">3</h1>
			</div>
			<div class="row">
				<h1 @click="handleDigits('4')">4</h1>
				<h1 @click="handleDigits('5')">5</h1>
				<h1 @click="handleDigits('6')">6</h1>
			</div>
			<div class="row">
				<h1 @click="handleDigits('7')">7</h1>
				<h1 @click="handleDigits('8')">8</h1>
				<h1 @click="handleDigits('9')">9</h1>
			</div>
		</div>
		<div class="action-box">
			<div class="actions" v-for="(action, index) in actions">
				<h1 class="action" @click="handleAction(action)">
					{{ action.symbol }}
				</h1>
			</div>
		</div>
	</main>
</template>

<style scoped>
	* {
		box-sizing: border-box;
	}
	.title {
		text-align: center;
		font-size: 32px;
	}
	.result {
		background-color: #333;
		height: 80px;
		width: 240px;
		color: white;
		font-size: 32px;
		font-weight: bold;
		text-align: right;
		padding-right: 10px;
	}
	.cal {
		display: flex;
	}
	.row {
		display: flex;
		margin: 0;
	}
	.row h1 {
		border: 1px solid black;
		background: white;
		height: 80px;
		width: 80px;
		font-size: 32px;
		color: black;
		display: flex;
		justify-content: center;
		align-items: center;
		cursor: pointer;
		margin: 0;
	}
	.row h1:hover {
		background-color: aqua;
	}

	.action {
		border: 1px solid black;
		color: black;
		display: flex;
		justify-content: center;
		align-items: center;
		cursor: pointer;
		width: 80px;
		height: 80px;
		background-color: burlywood;
		margin: 0;
	}

	.action-box {
		display: flex;
		flex-flow: column;
		margin: 0;
		height: 320px;
	}
</style>
