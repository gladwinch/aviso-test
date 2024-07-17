<template>
  <div class="block" :class="{'active': active }" @click=""></div>
</template>

<script setup>
const props = defineProps({
  active: {
    type: Boolean,
    required: true,
  },
});
</script>

<style scoped>
	.block {
		height: 8rem;
		width: 8rem;
		background-color: white;
		border: solid 1px black;
	}

	.active {
		background-color: green !important;
	}
</style>
