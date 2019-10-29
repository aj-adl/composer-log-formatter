<template>
  <div>
    <div class="section header">
		<div class="container">
		<h1 class="title">
			🌲Composer Log Splitter
		</h1>
		<h2 class="subtitle">
			Lets parse some logs
		</h2>
	  </div>
    </div>
	<div class="section">
		<log-input></log-input>
	</div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import LogInput from '~/components/LogInput.vue'

export default {
  components: {
	Logo,
	LogInput
  }
}
</script>

<style>
.section {
	background: linear-gradient(180deg,#f1f1f1,#fff 15rem,#fff 0,#fff);
}

.header {
	text-align: center;
	background: none;
	height: 32vw;
}

.header .container {
	display: flex;
	height: 100%;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
