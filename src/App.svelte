<script lang="ts">
	import { paginate, PaginationNav } from "svelte-paginate";
	import axios from "axios";
	import { onMount } from "svelte";

	interface Item {
		body: string;
		id: number;
		title: string;
		userId: number;
	}

	let items: Item[] = [];

	onMount(async () => {
		const res = await axios.get(
			"https://jsonplaceholder.typicode.com/posts"
		);
		items = res.data;
	});

	let currentPage = 1;
	let pageSize = 10;

	let paginatedItems: Item[];
	$: paginatedItems = paginate({ items, pageSize, currentPage });
</script>

<main>
	<ul>
		{#each paginatedItems as item}
			<li>{item.title}</li>
		{:else}
			<p>loading...</p>
		{/each}
	</ul>

	{#if items.length > 0}
		<nav class="pagination">
			<PaginationNav
				totalItems={items.length}
				{pageSize}
				{currentPage}
				limit={1}
				showStepOptions={true}
				on:setPage={(e) => (currentPage = e.detail.page)}
			>
				<button slot="prev" />
				<button slot="next" />
			</PaginationNav>
		</nav>
	{/if}
</main>

<style>
	.pagination :global(.pagination-nav) {
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: flex-start;
	}

	.pagination :global(.option) {
		display: inline-block;
		width: 2em;
		cursor: pointer;
		text-align: center;
		user-select: none;
	}

	.pagination :global(.option.active) {
		color: coral;
		position: relative;
	}

	.pagination :global(.option.active)::after {
		content: "";
		position: absolute;
		left: 50%;
		bottom: -2px;
		width: 16px;
		height: 2px;
		border-radius: 1px;
		opacity: 1;
		background-color: coral;
		transform: translateX(-50%);
		transition: opacity 0.04s linear 0.2s;
	}

	.pagination :global(.option.prev button) {
		margin-right: 4px;
		min-width: 28px;
		background-color: transparent;
		border: none;
		cursor: pointer;
		height: 24px;
		width: 24px;
		background-image: url("https://assets.tokopedia.net/assets-tokopedia-lite/v2/zeus/kratos/66729b52.svg");
		background-repeat: no-repeat;
		background-size: 24px;
		background-position: center center;
		transform: scaleX(-1);
	}

	.pagination :global(.option.next button) {
		margin-right: 4px;
		min-width: 28px;
		background-color: transparent;
		border: none;
		cursor: pointer;
		height: 24px;
		width: 24px;
		background-image: url("https://assets.tokopedia.net/assets-tokopedia-lite/v2/zeus/kratos/66729b52.svg");
		background-repeat: no-repeat;
		background-size: 24px;
		background-position: center center;
	}
</style>
