<template>
	<div id="app">
		<button class='btn' @click='getUserAgeFeatureStatus'>
			Calculate Age
		</button>
		
		<p v-if='!userAgeFeature && !loading' class='text'>Sorry, This feature has been disabled by the Admin</p>
		<p v-else-if="error">{error}</p>

		<div v-else-if='userAgeFeature'>
			<p class='text'>Calculate your age below by providing your year of birth</p>
			<input
				type='number'
				v-model='birthYear'
			/>
			<button class='btn btn-calculate' @click='calcAge'>
				Calculate
			</button>

			<p class="ageText" v-if='age'>You are {{ age }} years old</p>
		</div>
	</div>
</template>

<script>
import * as configcat from "configcat-js";

export default {
	name: 'App',
	data() {
		return {
			userAgeFeature: null,
			error: null,
			birthYear: '',
			age: '',
			loading: true
		};
	},
	methods: {
		// Get feature status from ConfigCat
		async getUserAgeFeatureStatus() {
      console.log("clicked")
			try {
				const configCatClient = configcat.getClient(
					"configcat-sdk-1/kfLcCLn2TU6C2QE4krHiew/4hglqXmsWk2vYS5GC6-M4w"
				);
				console.log({configCatClient})
				// Retrieve the feature flag value
				const res = await configCatClient.getValueAsync("calculateuseragefeature", false);
        console.log(res)
				this.userAgeFeature = res;
			} catch (err) {
				this.error = err.message;
			}
			this.loading = false;
		},
		// Calculate age feature
		calcAge() {
			const currentYear = new Date().getFullYear();
			this.age = currentYear - this.birthYear;
			this.birthYear = '';
		}
	}
};
</script>

<style>
body {
	margin: 0;
	padding:0;
}

#app {
	text-align: center;
	background:#10375C;
	min-height: 100vh;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.btn {
	color: #F4F6FF;
	  background-color: #EB8317;
	border: none;
	 padding: 8px 16px;
	border-radius: 15px;
	font-size: 1.2rem;
	cursor: pointer;
	font-weight: 600;
}

.btn:hover {
	background-color: #F3C623;
	color: #FFF;
}

.btn:focus-visible,
input:focus-visible {
	outline: none;
}

.btn-calculate {
	padding: 10px 25px;
	margin-left: 8px;
	border-radius: 8px;
}

input {
	padding: 10px 25px;
	font-size: 1.2rem;
	border-radius: 8px;
	border: none;
	background:#ffffff;
	color:#000000;
}

.text {
	font-size: 1.5em;
	color: #ffffff;
}
.ageText{
	color:#FF6500;
	font-weight:600;
}
</style>