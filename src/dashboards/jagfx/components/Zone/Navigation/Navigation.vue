<template>
	<div class="nav text-center">
		<div class="icons-item-wrappers">
			<div class="dash-symbol-area">
				<!-- Left elements -->
				<DashSymbolArea side="left"></DashSymbolArea>
				
				<div :class="telemetry.truck.transmission.shifterType" class="truck-gear">
					<span class="value">{{ $trukGear(telemetry.truck.transmission, telemetry.truck.brand) }}</span>
				</div>
				
				<!-- Right elements -->
				<DashSymbolArea side="right"></DashSymbolArea>
			</div>
			
			<!-- Speed -->
			
			<div class="speed-area">
				<div class="speed-wrapper">
					<div class="left" v-if="$elementIsEnabled( 'middle-b', 'jagfx_elements_mid_bottom_rpm' )">
						<RPMBars v-bind="{
							engine: telemetry.truck.engine,
							brand: telemetry.truck.brand,
							model: telemetry.truck.model
						}"></RPMBars>
					</div>
					<div class="middle">
						<div class="speed">
							<span class="value">{{ unit_speed( telemetry.truck.speed, true, false ) | $toFixed( 0 )  }}</span>
							<small class="unit">{{ unit_speed( telemetry.truck.speed, false ) }}</small>
						</div>
						
						<div class="fuel-level" v-if="$elementIsEnabled( 'middle-b','jagfx_elements_mid_bottom_fuel' )">
							<span class="fuel-icon-wrapper">
								<i :class="{'warning': onWarningLevel() }" class="icon-fuel"></i>
							</span>
							
							<div class="bars">
								<div :class="{ 'active' : getFuelBarActive( i ) }" class="bar" :key="i" v-for="i in getFuelBarCount()"></div>
							</div>
						</div>
					</div>
					<div class="right" v-if="$elementIsEnabled( 'middle-b', 'jagfx_elements_mid_bottom_rpm' )">
						<RPMBars v-bind="{
							engine: telemetry.truck.engine,
							brand: telemetry.truck.brand,
							model: telemetry.truck.model
						}"></RPMBars>
					</div>
				</div>
			</div>
			
			<!--<div class="speed">
				<span class="value">{{unit_speed(telemetry.truck.speed true, false )}}</span>
				<small class="unit">km/h</small>
			</div>-->
			
			<div class="odometer">
				<span class="value">{{ unit_length( telemetry.truck.odometer, 'km', true, false ) | $toFixed( 0 ) }}</span>
				<small class="unit">{{ unit_length( telemetry.truck.odometer, 'km', false ) }}</small>
			</div>
			
			<!-- Speed limit -->
			<div class="bottomArea">
				<div :class="{ 'active': telemetry.truck.lights.blinker.left.active}" v-if="$elementIsEnabled( 'middle-b', 'jagfx_elements_mid_bottom_blinker_turn' )" class="blinkers">
					<i class="icon-blinker_left"></i></div>
				<div :class="{ 'hidden': telemetry.navigation.speedLimit.value === 0 }" v-if="$elementIsEnabled( 'middle-b', 'jagfx_elements_mid_bottom_speedLimit' )" class="speedLimitKPH">
					{{ unit_speed( telemetry.navigation.speedLimit, true, false ) }}
				</div>
				<div :class="{ 'active': telemetry.truck.lights.blinker.right.active}" v-if="$elementIsEnabled( 'middle-b', 'jagfx_elements_mid_bottom_blinker_turn' )" class="blinkers">
					<i class="icon-blinker_right"></i></div>
				<!--<div class="speedLimitMPH">{{telemetry.navigation.speedLimit.mph}}</div>-->
			</div>
		
		</div>
		<!--<div>
			&lt;!&ndash;<div><b>Next rest stop:</b><div>{{formatNextRestStop()}}</div></div>
			<div><b>DistancIe:</b><div>{{(distance / 1000).toFixed().toLocaleString()}}km / {{(distance * 0.0006213712).toFixed()}}Miles</div></div>
			<div><b>ETA</b><div>{{formatETA()}}</div></div>&ndash;&gt;
			&lt;!&ndash;<div><b>Speed</b><div>{{telemetry.truck.speed unit_speed( telemetry.truck.speed, true, false )}}kph / {{telemetry.truck.speed.mph}}mph</div></div>&ndash;&gt;
		</div>-->
	</div>
</template>

<script>
	import AppDashMixins     from '../../../../../components/Mixins/AppDashMixins';
	import DashSymbolArea from '../../Elements/DashSymbolArea';
	import JagfxConfigMixins   from '../../Mixins/JagfxConfigMixins';
	import RPMBars        from './Elements/RPMBars';
	
	export default {
		name:       'Navigation',
		components: { DashSymbolArea, RPMBars },
		mixins:     [ AppDashMixins, JagfxConfigMixins ],
		methods:    {
			getFuelByBar:     function () {
				return (this.telemetry.truck.fuel.capacity * this.telemetry.truck.fuel.warning.factor).toFixed( 0 );
			},
			getFuelBarCount:  function () {
				return Math.ceil( this.telemetry.truck.fuel.capacity / this.getFuelByBar() );
			},
			getFuelBarActive: function ( i ) {
				const fuelByBar   = this.getFuelByBar();
				const iLow        = i - 1;
				const fuelBarFrom = (iLow * fuelByBar);
				
				//console.log( i, this.telemetry.truck.fuel.value, fuelBarFrom );
				return (this.telemetry.truck.fuel.value >= fuelBarFrom);
			},
			onWarningLevel:   function () {
				return this.telemetry.truck.fuel.value < this.getFuelByBar();
			}
		}
	};
</script>

<style scoped lang="scss">
	@import "../../../assets/scss/navigation/navigation";
</style>
