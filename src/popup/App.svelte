<script>
	import Test from "./components/Test.svelte";
	import "./global.css"
	const convertDate = (date) => {
		const dateArray = date.substring(0, 10).split(".")
		return `${dateArray[2]}/${dateArray[1]}/${dateArray[0]}`
	}

	let plate = ""
    let carData
	let motTests = []
	let fetchError = false

	const getData = async () => {
		try {
			motTests = []
			const res = await fetch(`https://odd-bull-sandals.cyclic.app/beta.check-mot.service.gov.uk/trade/vehicles/mot-tests?registration=${plate}`, {
				method: "GET",
				headers: {
					Accept: 'application/json+v6',
				  	"x-api-key": 'IK8c5HJa6p67EsUPaOryTe4KICoOdGa9yc2UAd00',
				},
			})
			const data = await res.json()
			fetchError = false
            carData = data[0]
			motTests = data[0].motTests
		} catch (error) {
			console.error(`Error: ${error}`)
			fetchError = true
		}
	}
</script>

<main>
	<div class="header__container">
		<header>
			<h1 class="title">MOT History</h1>
			<form on:submit|preventDefault={getData}>
				<label for="vrn">Registration number</label>
				<input type="text" bind:value={plate} />
				<button class="btn" type="submit">Search</button>
			</form>
		</header>
		{#if carData}
			<aside>
				<h2 class="car__name">{carData.make} {carData.model}</h2>
				<p class="car__detail">
					<span class="test__heading">Colour:</span>
					{carData.primaryColour}
				</p>
				<p class="car__detail">
					<span class="test__heading">Fuel Type:</span>
					{carData.fuelType}
				</p>
				<p class="car__detail">
					<span class="test__heading">Date Registered:</span>
					{convertDate(carData.firstUsedDate)}
				</p>
			</aside>
		{/if}
	</div>
	{#each motTests as test, i}
		<Test {test} />
	{/each}
	{#if fetchError}
		<h3 style="text-align: centre;">
			Someting went wrong. Try checking the registration number again.
		</h3>
	{/if}
</main>

<style>
</style>
