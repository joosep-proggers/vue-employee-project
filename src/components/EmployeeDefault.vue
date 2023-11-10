<script >
	export default {
		name: "EmployeeDefault",
		props: [
			"fname",
			"lname",
			"position",
			]
	}
</script>


<template>
	<div class="employee">
		<p class="employee-name"> {{ fname}} {{ lname }} </p>
	</div>
</template>

<style>
	.employee-name{
		cursor: pointer;
		margin: auto;
		width: max-content;
		font-size: 2vh;
	}
	.employee{
		margin: 1vh;
	}
</style>