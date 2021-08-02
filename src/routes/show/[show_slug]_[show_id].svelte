<script context="module">
	export async function load({ page,fetch}) {
		const title = page.params.show_slug;
		const id = page.params.show_id;

		const show_raw = await fetch(`https://api.themoviedb.org/3/tv/${id}?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&adult=true&include_image_language=en,null&append_to_response=videos,images,credits,similar,recommendations,external_ids,keywords,translations,alternative_titles,content_ratings,reviews`);
		const show_json = await show_raw.json();

		return {
			props: {
				title,
				show_seasons:show_json.seasons,
				show_id:show_json.id,
				show:show_json,
			}
		};
	}
</script>

<svelte:head>
  <title>Tv Space - {slugToText(title)}</title>
</svelte:head>

<script>
	import Card from '$lib/Card.svelte';

	import Title from '$lib/Route_title.svelte';
	import { Tabs, Tab, TabList, TabPanel } from 'svelte-tabs';
	import { Accordion, AccordionItem } from "svelte-accessible-accordion";
	import { onMount } from 'svelte';
	import Select from 'svelte-select';

	import ShowInfoBox from '$lib/ShowInfoBox.svelte';

	export let title;
	export let show_seasons;
	export let show_id;
	export let show;

	let selctedSeason =1;
	let selctedEpisode = 1;
	let season = 1;
	let episode = 1;

	function rn() {
		selctedSeason = 1;
		selctedEpisode =1;
  		selctedSeason = +this.options[this.selectedIndex].getAttribute('data-season');
  		selctedEpisode = +this.options[this.selectedIndex].getAttribute('data-episode');
	}


	function slugToText(slug) {
	  var words = slug.split('-');

	  for (var i = 0; i < words.length; i++) {
	    var word = words[i];
	    words[i] = word.charAt(0).toUpperCase() + word.slice(1);
	  }

	  return words.join(' ');
	}
	function epi(count) {
		let countArray = [];
		for (var input = 1; input <= count; input++) {
	 		countArray.push(input)
		}
	 	return countArray;
	}
	async function getEpisode(seasonNo,seasonId) {
		const epi_raw = await fetch(`https://api.themoviedb.org/3/tv/${seasonId}/season/${seasonNo}?api_key=04c35731a5ee918f014970082a0088b1&language=en-US`);
		const epi_json = await epi_raw.json();
		return epi_json.episodes;
	}
	function formatDate(userDOB) {
	  const dob = new Date(userDOB);

	  const monthNames = [
	    'January', 'February', 'March', 'April', 'May', 'June', 'July',
	     'August', 'September', 'October', 'November', 'December'
	  ];

	  const day = dob.getDate();
	  const monthIndex = dob.getMonth();
	  const year = dob.getFullYear();

	  return `${day} ${monthNames[monthIndex]} ${year}`;
	}
</script>

<Title>
	<span class="page-title">	
		{slugToText(title)}
	</span>
</Title>

<div class="player">
	<div class="player-frame">
		<iframe width="100%" height="100%" class="metaframe" src="https://www.2embed.ru/embed/tmdb/tv?id={show_id}&s={selctedSeason}&e={selctedEpisode}" frameborder="0" scrolling="no" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen" webkitallowfullscreen="true" mozallowfullscreen="true"></iframe>
	</div>
		<div class="selection">
			<select on:change={rn} class="season-select">
				{#each show_seasons as seasonCount}
				  <optgroup label="{seasonCount.name}">
							{#await getEpisode(seasonCount.season_number,show_id)}
								<p>No Seasons & Episodes</p>
							{:then nam}
				  				{#each nam as coint}
								    <option
								    	title="{coint.name}: {coint.overview}" 
								    	value="{coint.episode_number}" 
								    	data-season={coint.season_number} 
								    	data-episode={coint.episode_number}> S{coint.season_number}-E{coint.episode_number} - {coint.name} 
								    </option>
								{/each}
							{/await}
				  </optgroup>
				{/each}
			</select>
		</div>
</div>


<ShowInfoBox {show} />

<div class="overview-container">
	<div class="over-view">
		<h2>overview</h2>
		<br/>
		{#if show.overview}
			<p>{show.overview}</p>
		{:else}
			<p class="not-found">No Overview Found...Sorry</p>
		{/if} 
	</div>

	<div class="similar-movies">
		<h2>similar shows</h2>
		<div class="similar-list">
			{#each show.similar.results as video}
		  			<Card vote={video.vote_average} language="{video.original_language}" id={video.id} overview="{video.overview}" posterPath="{video.poster_path}" name="{video.name}" />
			{/each}
		</div>
	</div>
	{#if show.recommendations.results}
		<div class="similar-movies">
			<h2>recommended shows</h2>
			<div class="similar-list">
				{#each show.recommendations.results as video}
			  			<Card vote={video.vote_average} language="{video.original_language}" id={video.id} overview="{video.overview}" posterPath="{video.poster_path}" name="{video.name}" />
				{/each}
			</div>
		</div>
	{/if}
<!-- 	<div class="key-words">
		<h2>keywords</h2>
		<div class="word-list">
			{#each show.keywords.results as key}
				<a class="keyword-pill">{key.name}</a>
			{:else}
				<p class="not-found">No keywords Found...Sorry</p>
			{/each}
		</div>
	</div> -->
</div>


<style>
	.not-found{
		width: 100%;
		padding-bottom: 1rem;
		text-align: center;
		opacity: 0.6;
		color: #DADADA;
		font-size: 0.95rem;
		font-style: oblique;
		text-transform: capitalize;
	}
	.overview-container{
		display: flex;
		align-items: center;
		flex-direction: column;
		line-height: 1.6;
		margin: 1rem auto;
		width: 90%;
	}
	.similar-movies{
		width: 97.5%;
		min-height: 4rem;
		padding: 1rem;
		background-color: #192129;
		margin-bottom: 1rem;
	}
	.similar-list{
		display: flex;
		overflow-x: overlay;
	}
	.over-view{
		width: 97.5%;
		background-color: #192129;
		padding: 1rem;
		font-size: 1.05rem;
		margin-bottom: 1rem;
		min-height: 4rem;
	}
	.key-words{
		width: 97.5%;
		min-height: 4rem;
		padding: 1rem;
		font-size: 1.05rem;
		background-color: #192129;
		margin-bottom: 1rem;
	}
	.word-list{
		display: flex;
		align-items: center;
		flex-wrap: wrap;
	}
	.keyword-pill{
		display: block;
		padding:0.3rem 0.8rem;
		background-color: #0d1216;
		margin: 0.3rem;
		border-radius:10px;
		font-size: 1rem;
		cursor: pointer;
	}
	.keyword-pill:hover{
		background-color: #ffdf00;
		color: #0d1216;
		font-weight: 500;
	}
	.over-view h2,.key-words h2,.similar-movies h2{
		display: inline-block;
		background-color: #ffdf00;
		text-transform: uppercase;
		color: #192129;
		font-weight: bold;
		padding: 0 0.3rem;
		margin-bottom: 0.7rem;
	}
	.page-title{
		font-size: 2rem;
	}
	.player{
		margin-bottom: 2rem;
		width: 90%;
		margin: 0 auto;
	}
	.player-frame{
		margin-bottom: 1.5rem;
		height: 35rem;
		background-color: #192129;

	}
	iframe{
		width: 100%;
		height: 100%;
	}
	.selection{
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 2rem;
	}
	.season-select{
		width: 25rem;
		font-size: 1.1rem;
		font-family: inherit;
		color: #DADADA;
		border: 0;
		outline: 0;
		padding: 0.5rem 1rem;
		background-color: #192129;
		margin-left: 1rem;
	}	
	.season-select optgroup{
		width: 100%;
	}
	.similar-list::-webkit-scrollbar {
		height: 0.5rem;
	}

	.similar-list::-webkit-scrollbar-track {
		color: #ffdf00;
	  	box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
	  	background-color: #192129;
	}
	.similar-list::-webkit-scrollbar-thumb:hover{
	  	background-color: #ffdf00;
	}
	.similar-list::-webkit-scrollbar-thumb {
		border-radius:1rem;
	  	background-color: darkgray;
	}
</style>