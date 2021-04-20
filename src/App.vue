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
					<div :style="monsterBarStyles" class="healthBar__value" />
				</div>
			</section>
			<section id="player" class="container">
				<h2>Your Health</h2>
				<div class="healthBar">
					<div :style="playerBarStyles" class="healthBar__value" />
				</div>
			</section>
			<section v-if="winner" class="container">
				<h2>Game Over!</h2>
				<h3 v-if="winner === 'player'">You won!</h3>
				<h3 v-if="winner === 'monster'">You lost!</h3>
				<h3 v-if="winner === 'draw'">It's a draw!</h3>
				<button @click="startNewGame">Start new game</button>
			</section>
			<section v-else id="controls">
				<button @click="attackMonster">ATTACK</button>
				<button
					:disabled="canUseSpecialAttackMonster"
					@click="specialAttackMonster"
				>
					SPECIAL ATTACK
				</button>
				<button @click="healPlayer">HEAL</button>
				<button @click="surrender">SURRENDER</button>
			</section>
			<section id="log" class="container">
				<h2>Battle Log</h2>
				<ul>
					<li v-for="(logMessage, index) in logMessages" :key="index">
						<span
							:class="{
								'log--player': logMessage.actionBy === 'player',
								'log--monster': logMessage.actionBy === 'monster'
							}"
						>
							{{ logMessage.actionBy === 'player' ? 'Player' : 'Monster' }}
						</span>
						<span v-if="logMessage.actionType === 'heal'">
							heals himself for
							<span class="log--heal">{{ logMessage.actionValue }}</span>
						</span>
						<span v-else>
							attacks and deals
							<span class="log--damage">{{ logMessage.actionValue }}</span>
						</span>
					</li>
				</ul>
			</section>
		</div>
	</div>
</template>

<script>
/*
import HelloWorld from '@/components/HelloWorld.vue';
*/

const getRandomValue = (min, max) =>
	Math.floor(Math.random() * (max - min)) + min;

export default {
	name: 'App',
	components: {
		/*
		HelloWorld
		*/
	},
	data: () => ({
		playerHealth: 100,
		monsterHealth: 100,
		currentRound: 0,
		winner: null,
		logMessages: []
	}),
	computed: {
		canUseSpecialAttackMonster() {
			return this.currentRound % 3 !== 0;
		},
		monsterBarStyles() {
			if (this.monsterHealth < 0) {
				return { width: '0%' };
			}

			return { width: this.monsterHealth + '%' };
		},
		playerBarStyles() {
			if (this.playerHealth < 0) {
				return { width: '0%' };
			}

			return { width: this.playerHealth + '%' };
		}
	},
	watch: {
		monsterHealth(value) {
			if (value <= 0 && this.playerHealth <= 0) {
				// A draw
				this.winner = 'draw';
			} else if (value <= 0) {
				// Monster lost
				this.winner = 'player';
			}
		},
		playerHealth(value) {
			if (value <= 0 && this.monsterHealth <= 0) {
				// A draw
				this.winner = 'draw';
			} else if (value <= 0) {
				// Player lost
				this.winner = 'monster';
			}
		}
	},
	methods: {
		attackMonster() {
			this.currentRound++;

			const attackValue = getRandomValue(5, 12);

			this.monsterHealth -= attackValue;

			this.addLogMessage('player', 'attack', attackValue);

			this.attackPlayer();
		},
		attackPlayer() {
			const attackValue = getRandomValue(8, 15);

			this.addLogMessage('monster', 'attack', attackValue);

			this.playerHealth -= attackValue;
		},
		specialAttackMonster() {
			this.currentRound++;

			const attackValue = getRandomValue(10, 25);

			this.monsterHealth -= attackValue;

			this.addLogMessage('player', 'attack', attackValue);

			this.attackPlayer();
		},
		healPlayer() {
			this.currentRound++;

			const healValue = getRandomValue(8, 20);

			if (this.playerHealth + healValue > 100) {
				this.playerHealth = 100;
			} else {
				this.playerHealth += healValue;
			}

			this.addLogMessage('player', 'heal', healValue);

			this.attackPlayer();
		},
		surrender() {
			this.winner = 'monster';
		},
		startNewGame() {
			this.winner = null;
			this.currentRound = 0;
			this.playerHealth = 100;
			this.monsterHealth = 100;
		},
		addLogMessage(actionBy, actionType, actionValue) {
			this.logMessages.unshift({
				actionBy,
				actionType,
				actionValue
			});
		}
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
