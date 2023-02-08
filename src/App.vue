<script>
export default {
	data() {
		return {
			min_tens: 0,
			min: 0,
			sec_tens: 0,
			sec: 0,
			interval: null,
			inputData: 10
		}
	},
	methods: {
		btnClick(event) {
			if (!this.interval) {
				event.target.innerText = 'Stop Timer'
				this.startTimer(this.inputData)
			} else {
				event.target.innerText = 'Start Timer'
				this.clearTimer()
			}
		},
		setTimer({ min_tens, min, sec_tens, sec }) {
			this.min_tens = min_tens
			this.min = min
			this.sec_tens = sec_tens
			this.sec = sec
		},
		clearTimer() {
			if (this.interval) clearInterval(this.interval)
			this.interval = null
			this.setTimer({
				min_tens: 0,
				min: 0,
				sec_tens: 0,
				sec: 0
			})
		},
		startTimer(time) {
			this.clearTimer()
			const end = Date.now() + time * 1000 * 60
			this.interval = setInterval(() => {
				const now = Date.now()
				const delta = end - now
				if (delta < 0) {
					clearInterval(this.interval)
					return
				}
				this.setTimer({
					min_tens: Math.floor(delta / 1000 / 60 / 10),
					min: Math.floor((delta / 1000 / 60) % 10),
					sec_tens: Math.floor((delta % 60000) / 10000),
					sec: Math.floor(((delta % 60000) / 1000) % 10)
				})
			}, 500)
		}
	}
}
</script>

<template>
	<div class="wrapper">
		<div class="timer">
			<div class="info">
				Set the timer to
				<input
					type="number"
					name="time"
					aria-label="Meditation minutes"
					v-model="inputData"
				/>
				minutes
			</div>
			<div class="countdown">
				<div class="num">{{ min_tens }}</div>
				<div class="num">{{ min }}</div>
				<div class="dots">:</div>
				<div class="num">{{ sec_tens }}</div>
				<div class="num">{{ sec }}</div>
			</div>
			<button class="btn" @click="btnClick">Start Timer</button>
		</div>
	</div>
</template>

<style>
.wrapper {
	background: #202024;
	border-radius: 8px;
	padding: 60px 250px;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	gap: 50px;
}

.timer {
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	gap: 30px;
}

.info {
	font-size: 18px;
	line-height: 160%;
	font-weight: 700;
}

.info input {
	width: 40px;
	background: none;
	border: none;
	text-align: center;
	font-weight: 700;
	color: var(--anti-accent);
	font-size: 18px;
	padding: 5px 10px;
}

.info input::-webkit-outer-spin-button,
.info input::-webkit-inner-spin-button {
	-webkit-appearance: none;
	margin: 0;
}

.info input[type='number'] {
	appearance: textfield;
}

.countdown {
	display: flex;
	gap: 16px;
	align-items: center;
}

.num {
	font-family: 'Roboto Mono', monospace;
	font-size: 160px;
	line-height: 1;
	background: #29292e;
	padding: 40px 16px;
	border-radius: 8px;
}

.dots {
	font-family: 'Roboto Mono', monospace;
	font-size: 160px;
	color: var(--primary);
}

.btn {
	background: var(--primary);
	border: none;
	border-radius: 8px;
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 16px;
	font-weight: 700;
	color: white;
	gap: 10px;
	padding: 17px;
	width: 100%;
	cursor: pointer;
}
</style>
