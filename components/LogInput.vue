<template>
<div class="container logParser has-text-left">

	<div class="field">
		<label for="log-input" class="label">Composer Output</label>
		<textarea v-model="input" class="textarea" name="log-input" id="" cols="30" rows="10"></textarea>
	</div>

	<div class="field">
		<h3 class="label">Reporting Output</h3>
		<div class="box content">
			<strong>WordPress Core</strong>
			<ul>
				<li v-for="item in this.coreUpdates">{{ format( item.name ) }} - {{ item.previousVersion }} => {{ item.currentVersion }}</li>
				<li v-if="!this.coreUpdates.length">No WordPress Core Upgrades this month </li>
			</ul>
			<strong>Plugin Updates</strong>
			<ul>
				<li v-for="item in this.pluginUpdates">{{ format( item.name ) }} - {{ item.previousVersion }} => {{ item.currentVersion }}</li>
				<li v-if="!this.pluginUpdates.length">No Plugin Updates this month </li>
			</ul>
			<strong>Library Updates</strong>
			<ul>
				<li v-for="item in this.libUpdates">{{ format( item.name ) }} - {{ item.previousVersion }} => {{ item.currentVersion }}</li>
				<li v-if="!this.libUpdates.length">No Library Updates this month </li>
			</ul>
		</div>
	</div>
	<div class="field">
		<h4 class="label">Raw Data</h4>
		<code v-html>{{ parsedLogs }}</code>
	</div>
</div>
</template>

<style>
	.logParser {
		padding: 3rem 3rem;
	}

	.logParser > .field {
		padding-bottom: 2rem;
	}
</style>

<script>
export default {
	name : 'log-input',

	data: function() {
		return {
		input : ''
		}
	},

	methods: {
		format(string){
			
			const chars = /[\_\-]/gi;

			return this.titleCase( string.replace( chars, ' ') );

		},

		titleCase(string) {
			return string.toLowerCase().split(' ').map(function(word) {
				return (word.charAt(0).toUpperCase() + word.slice(1));
			}).join(' ');
		}
	},

	computed: {

		matches(){
			const regex = / - Updating|Upgrading ([a-zA-Z0-9\-]*)\/([a-zA-Z0-9\-]*) \(([a-z0-9\.]*) => ([a-z0-9\.]*)/gm;
			let matches = [];

			for( const match of this.input.matchAll( regex ) ) {
				var whole,vendor,name,previousVersion, currentVersion;

				[ whole,vendor,name,previousVersion, currentVersion ] = match;

				matches.push( {
					vendor: vendor,
					name: name,
					previousVersion: previousVersion,
					currentVersion: currentVersion,
				} );
			}


			return matches;
		},

		coreUpdates(){
			const coreNames = [ 'wordpress', 'wordpress-core' ];

			let allUpdates = this.matches.filter( ( item ) => coreNames.includes( item.name ) );

			if ( allUpdates.length ) return [ allUpdates[0] ];

			return [];
		},

		pluginUpdates(){
			const pluginVendors = [ 'wpackagist-plugin', 'frame-plugin', 'frame-woocommerce' ];

			return this.matches.filter( ( item ) => pluginVendors.includes( item.vendor ) );
		},

		libUpdates(){
			const excludedvendors = [ 'johnpbloch', 'wpackagist-plugin', 'frame-plugin', 'frame-woocommerce' ];

			return this.matches.filter( ( item ) => ! excludedvendors.includes( item.vendor ) );
		},

		parsedLogs(){
			return JSON.stringify( {
				core: this.coreUpdates,
				plugins: this.pluginUpdates,
				libs: this.libUpdates,
			} );
		}

	}




}
</script>

