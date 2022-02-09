<template>
	<div id="app">
		<input type="text" class="search" @input="fnSearch($event.target.value)">
		<hr>
		<br>
		<div class="content-list" v-if="!isloading">
			<div class="item" v-for=" item in itemList " :key="item.uuid4">
				<img :src=" item.image" alt="">
				<p v-text="'Nombre: ' + item.name.esp"></p>
				<p v-text=" 'Precio: ' + '$' +   item.suggested_budget "></p>
				<p v-text="fnGetCategory(item.category_type)"></p>
			</div> 
			<p class="no-data" v-if="itemList.length == 0 ">No hay datos</p>
		</div>
		<img src="@/assets/loading.gif" class="loading" v-else alt="loading">
	</div>
</template>

<script>
	export default {
		name: 'App',
		components: {
		},
		data: () => ({
			tempList: [],
			itemList: [],
			isloading: true,
		}),
		methods: {
			fnGetCategory(categoryId) {
				switch (categoryId) {
					case 1:
						return 'Normal';
					case 2:
						return 'Libre';
					case 3:
						return 'Personalizada';
					default:
						return '-';
				}
			},
			async fnApiGetData() {
				const headers = {
					Authorization: '5bc95bf034d900548243a59e2296bd683729bd75057879fd0f877d3adc7d1db6bedbfccb47aca04e44ef28adf4c3e9e72afe2f2b295b3bf08e2a47ec75f9607d',
					'Content-Type': 'application/json',

				};
				console.log('Geting data');
				this.isloading = true;
				await fetch('https://apitesting.plerk.io/v2/category', {
						headers
					})
					.then(response => response.json())
					.then(response => {
						this.tempList = response.data || [];
						this.itemList = [...this.tempList];
					}).catch(err => console.log(err.code));
				this.isloading = false;

			},
			fnSearch(value) {
				if (value && value !== '') {
					this.itemList = this.tempList.filter(item =>  item.name.esp.toLowerCase()?.includes(value.toLowerCase()))
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
		font-size: 24px;
		border: 2px solid rgba(0, 0, 0, .2);
		box-shadow: 0px 5px 10px rgba(0, 0, 0, .1);
		margin-bottom: 30px;
		border-radius: 8px;
		padding: 0.5rem 1rem;
	}
	.search:focus {
		outline: none !important;
		border:1px solid rgba(0, 0, 0, .5);
		box-shadow: 0px 5px 10px rgba(0, 0, 0, .5);
	}

	.content-list {
		display: flex;
		flex-direction: row;
		flex-flow: row wrap;
		justify-content: space-between;
		width: 100%;
	}

	.item {
		width: 27.5%;
		margin-bottom: 1rem;
		padding: 1.5rem 0.5rem;
		border: 2px solid rgba(0, 0, 0, .2);
		background-color: white;
		box-shadow: 5px 5px 10px rgba(0, 0, 0, .1);
		border-radius: 10px;
	}

	.item>img {
		width: 100px;
		height: 100px;
		border-radius: 15px;
		background-color: gray;
		object-fit: cover;
		border: 1px solid rgba(0, 0, 0, .5);
		box-shadow: 2px 2px 5px rgba(0, 0, 0, .1);
	}

	.loading {
		height: 300px;
		width: 300px;
	}

	.no-data{
		width: 100%;
		text-align: center;
		font-size: 34px;
		font-weight: bold;
	}

	@media (max-width: 600px) {
		.item {
			width: 100%;
			padding: 1rem;
			margin-bottom: 1rem ;
		}
	}


	@media ( min-width: 600px) and ( max-width: 900px) {
		.item {
			width: 43%;
		}
	}


</style>