<script>
import CommentList from "./CommentList.svelte"

    export let test
	console.log(test)
	let {testResult: outcome, completedDate: testDate, expiryDate, odometerValue: mileage, rfrAndComments: comments} = test
	
	const convertDate = (date) => {
		const dateArray = date.substring(0, 10).split(".")
		return `${dateArray[2]}/${dateArray[1]}/${dateArray[0]}`
	}

	let advisories = comments.filter(comment => comment.type === "ADVISORY" || comment.type === "USER ENTERED")

	let minors = comments.filter(comment => {
		return comment.type === "MINOR"
	})

	let fails = comments.filter(comment => comment.type === "FAIL" || comment.type === "PRS" || comment.type === "MAJOR" || comment.type === "DANGEROUS")
</script>

<section class="test__container">
	<hr />
	<div class="details__container">
		<div class="test__detail">
			{#if outcome === "PASSED"}
				<p class="pass">PASS</p>
			{:else}
				<p class="fail">FAIL</p>
			{/if}
		</div>
		<div class="test__details">
			<div class="test__detail">
				<span class="test__heading">Test Date:</span>
				<p class="test__detail">{convertDate(testDate)}</p>
			</div>
			<div class="test__detail">
				<span class="test__heading">Mileage:</span>
				<p class="test__detail">{mileage} miles</p>
			</div>
			{#if outcome === "PASSED"}
				<div class="test__detail">
					<span class="test__heading">Expiry Date:</span>
					<p class="test__detail">{convertDate(expiryDate)}</p>
				</div>
			{/if}
		</div>
	</div>
	<div class="issues__container">
		<CommentList type="Reasons for Failure" comments={fails} />
		<CommentList type="Minor Defects" comments={minors} />
		<CommentList type="Advisories" comments={advisories} />
	</div>
</section>
