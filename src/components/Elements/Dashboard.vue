<template>
	<div :class="telemetry.truck.brand.id">
		<div id="overlay" v-show="enabledFullScreen()"></div>
		<slot v-bind="{ skinData: skinData() }"></slot>
	</div>
</template>

<script>
	import { mapGetters } from 'vuex';
	import AppDashMixins     from '../Mixins/AppDashMixins';
	
	export default {
		name:    'Dashboard',
		mixins:  [ AppDashMixins ],
		methods: {
			enabledFullScreen() {
				return process.env.VUE_APP_ENABLE_FULLSCREEN === 'true';
			},
			/*telemetryData() {
			 return this.pickData()();
			 },*/
			skinData() {
				return this.currentSkin();
			},
			...mapGetters( {
				//pickData:    'telemetry/pick',
				currentSkin: 'skins/current'
			} )
		}
	};
</script>

<style lang="scss" scoped>
	@import "../../assets/scss/common/dashboard";
</style>
