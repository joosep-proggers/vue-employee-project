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
	<div>
		<h2 class="employee-name"> {{ fname}} {{ lname }} </h2>
	</div>
</template>

<style>
	.employee-name{
		cursor: pointer;
		margin: 1vw;
	}
</style>