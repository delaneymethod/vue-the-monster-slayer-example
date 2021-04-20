<template>
	<div id="app">
		<!--
		<img alt="Vue logo" src="./assets/logo.png" />
		<HelloWorld msg="Welcome to Your Vue.js App" />
		-->
		<header>
			<h1>Monster Slayer</h1>
		</header>
		<div id="game">
			<section id="monster" class="container">
				<h2>Monster Health</h2>
				<div class="healthBar">
					<div class="healthBar__value"></div>
				</div>
			</section>
			<section id="player" class="container">
				<h2>Your Health</h2>
				<div class="healthBar">
					<div class="healthBar__value"></div>
				</div>
			</section>
			<section id="controls">
				<button>ATTACK</button>
				<button>SPECIAL ATTACK</button>
				<button>HEAL</button>
				<button>SURRENDER</button>
			</section>
			<section id="log" class="container">
				<h2>Battle Log</h2>
				<ul></ul>
			</section>
		</div>
	</div>
</template>

<script>
/*
import HelloWorld from '@/components/HelloWorld.vue';
*/

export default {
	name: 'App',
	components: {
		/*
		HelloWorld
		*/
	}
};
</script>

<style lang="scss">
/*
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
*/
</style>
