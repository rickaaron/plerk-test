<template>
	<div id="app">
		<input type="text" placeholder="Buscar..." class="search" @input="fnSearch($event.target.value)">
		<hr>
		<br>
		<div class="content-list" v-if="!isloading">
			<Category :item="item" v-for=" item in itemList " :key="item.uuid4" />
			<p class="no-data" v-if="itemList.length == 0 ">No hay datos</p>
		</div>
		<img src="@/assets/loading.gif" class="loading" v-else alt="loading">
	</div>
</template>

<script>
	import Category from '@/components/Category'
	export default {
		name: 'App',
		components: {
			Category
		},
		data: () => ({
			tempList: [],
			itemList: [],
			isloading: true,
		}),
		methods: {
			fetchTime(url, options, timeout = 10000) {
				return Promise.race([
					fetch(url, options),
					new Promise((_, reject) =>
						setTimeout(() => reject(new Error('timeout')), timeout)
					)
				]);
			},
			async fnApiGetData() {
				const headers = {
					Authorization: '5bc95bf034d900548243a59e2296bd683729bd75057879fd0f877d3adc7d1db6bedbfccb47aca04e44ef28adf4c3e9e72afe2f2b295b3bf08e2a47ec75f9607d',
					'Content-Type': 'application/json',
				};
				this.isloading = true;
				await this.fetchTime('https://apitesting.plerk.io/v2/category', {
						headers
					})
					.then(response => {
						if (response.status == 200) {
							return response.json();
						} else {
							alert('Algo salió mal, vuelvelo a intentar mas tarde.');
						}
					})
					.then(response => {
						this.tempList = response.data || [];
						this.itemList = [...this.tempList];
					}).catch(err => {
						console.table(err);
						if (err.message == 'timeout') {
							alert('Tiempo de espera superado, vuélvelo a intentar mas tarde');
						}
					});
				this.isloading = false;

			},
			fnSearch(value) {
				if (value && value !== '') {
					this.itemList = this.tempList.filter(item => item.name.esp.toLowerCase().includes(value
						.toLowerCase()))
				} else {
					this.itemList = [...this.tempList];

				}
			}
		},
		mounted() {
			this.fnApiGetData();
		}
	}
</script>


<style>
	* {
		box-sizing: border-box;
	}

	body {
		background: rgba(0, 0, 0, .05);
	}

	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
		padding: 0 2rem 0 2rem;
	}

	.search {
		width: 300px; 
		font-size: 24px;
		margin-bottom: 30px;
		border-radius: 8px;
		padding: 0.5rem 1rem;
		outline: none !important;
		border: none;
		box-shadow: 0px 5px 10px rgba(0, 0, 0, .1);
		transition: all 0.5s;
	}

	.search:focus {
		outline: none !important;
		border: 1px solid rgba(0, 0, 0, .5);
		box-shadow: 0px 5px 10px rgba(0, 0, 0, .5);
	}

	.content-list {
		display: flex;
		flex-direction: row;
		flex-flow: row wrap;
		justify-content: space-between;
		width: 100%;
	}



	.loading {
		height: 300px;
		width: 300px;
	}

	.no-data {
		width: 100%;
		text-align: center;
		font-size: 34px;
		font-weight: bold;
	}

	@media (max-width: 600px) {
		.search {
			width: 100%; 
		}
	}
</style>