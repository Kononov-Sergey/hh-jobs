<script lang="ts">
	import type {
		VacancyAddress,
		VacancyArea,
		VacancyEmployer,
		VacancySchedule
	} from '$lib/fetchers/get-vacancies';
	import CityBadge from '$lib/ui/city-badge.svelte';
	import DayAgo from '$lib/ui/day-ago.svelte';
	import ScheduleBadge from '$lib/ui/schedule-badge.svelte';
	import { fly } from 'svelte/transition';

	export let id: string;
	export let companyInfo: VacancyEmployer;
	export let title: string;
	export let publishedAt: string;
	export let address: VacancyAddress | null;
	export let area: VacancyArea;
	export let schedule: VacancySchedule | null;
	export let index: number;
</script>

<li class="wrapper" transition:fly={{ y: 200, duration: 1000, delay: 500 + index * 100 }}>
	{#if companyInfo.logo_urls}
		<img src={companyInfo.logo_urls[240]} alt={companyInfo.name} width="90" height="90" />
	{:else}
		<div class="mock-image">not found</div>
	{/if}

	<div class="main-info">
		<button class="company-btn" type="button">{companyInfo.name}</button>
		<a class="title-link" href="/job/{id}">{title}</a>
		{#if schedule}
			<ScheduleBadge titleId={schedule.id} />
		{/if}
	</div>
	<div class="additional-info">
		<CityBadge city={address?.city} country={area.name} />
		<DayAgo {publishedAt} />
	</div>
</li>

<style lang="scss">
	.wrapper {
		background-color: #fff;
		box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.05);
		border-radius: 4px;
		display: flex;
		gap: 16px;
		padding: 12px;
		img {
			width: 90px;
			height: 90px;
			min-width: 90px;
			min-height: 90px;
			border-radius: 4px;
			object-fit: contain;
		}
	}
	.mock-image {
		width: 90px;
		height: 90px;
		min-width: 90px;
		min-height: 90px;
		border-radius: 4px;
		background-color: #f2f2f2;
		color: #bdbdbd;
		font-family: 'Roboto';
		font-weight: 500;
		font-size: 12px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.main-info {
		color: #334680;
		font-family: 'Roboto';
		.company-btn,
		.title-link {
			color: #334680;
			font-family: 'Roboto';
			padding: 0px;
			&:hover {
				text-decoration: underline;
			}
		}
		.company-btn {
			font-weight: 700;
			font-size: 12px;
			margin-bottom: 8px;
		}
		.title-link {
			text-decoration: none;
			display: block;
			font-weight: 400;
			font-size: 18px;
			margin-bottom: 12px;
		}
	}
	.additional-info {
		flex-grow: 1;
		display: flex;
		justify-content: end;
		align-items: flex-end;
		gap: 29px;
	}
</style>
