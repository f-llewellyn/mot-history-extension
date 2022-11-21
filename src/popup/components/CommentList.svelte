<script>
    export let type, comments
</script>

{#if comments.length !== 0}
	<section class="comment__container">
		<span class="test__heading">{type}:</span>
		<ul class="issues__list">
			{#each comments as comment}
				<li>{comment.text}</li>
			{/each}
		</ul>
	</section>
{/if}
