<script context="module">
	export async function load({ page,fetch}) {

		let url,shows_raw,shows_json,currentData;

		if(page.params.stream_list === "now-streaming"){
			url = `https://api.themoviedb.org/3/tv/on_the_air?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1`;
			shows_raw = await fetch(url);
			shows_json = await shows_raw.json();
			currentData = "now-streaming"
		}else if(page.params.stream_list === "today-shows"){
			url=`https://api.themoviedb.org/3/tv/airing_today?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1`;
			shows_raw = await fetch(url);
			shows_json = await shows_raw.json();
			currentData = "today-shows"
			
		}else if(page.params.stream_list === "popular-shows"){
			url = `https://api.themoviedb.org/3/tv/popular?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1`
			shows_raw = await fetch(url);
			shows_json = await shows_raw.json();
			currentData ="popular-shows"

		}else if(page.params.stream_list === "top-shows"){
			url = `https://api.themoviedb.org/3/tv/top_rated?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1`
			shows_raw = await fetch(url);
			shows_json = await shows_raw.json();
			currentData = "top-shows"
		}else if(page.params.stream_list === "trending-shows"){
			url = `https://api.themoviedb.org/3/trending/tv/week?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1`
			shows_raw = await fetch(url);
			shows_json = await shows_raw.json();
			currentData = "trending-shows"
		}



		return {
			props: {
				currentData,
				currentPage: shows_json.page,
			    results: shows_json.results,
			    total: shows_json.total_results,
	      		totalPage: shows_json.total_pages,
			}
		};
	}
</script>




<script>
	import Title from '$lib/Route_title.svelte';
	import Card from '$lib/Card.svelte';
	import Spinner from '$lib/Spinner.svelte';
	import {slide} from 'svelte/transition';

	export let currentData;
	export let currentPage;
	export let results;
	export let total;
	export let totalPage;


  const load_more = async () => {
  	if(currentData === "now-streaming"){
		currentPage += 1;
		const res = await fetch(`https://api.themoviedb.org/3/tv/on_the_air?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=${currentPage}`);
		const raw_movie_data = await res.json();

		results = results.concat(raw_movie_data.results);
  	}else if(currentData === "today-shows"){
  		currentPage += 1;
		const res = await fetch(`https://api.themoviedb.org/3/tv/airing_today?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=${currentPage}`);
		const raw_movie_data = await res.json();

		results = results.concat(raw_movie_data.results);
  	}else if(currentData === "popular-shows"){
  		currentPage += 1;
		const res = await fetch(`https://api.themoviedb.org/3/tv/popular?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=${currentPage}`);
		const raw_movie_data = await res.json();

		results = results.concat(raw_movie_data.results);
  	}else if(currentData === "top-shows"){
  		currentPage += 1;
			const res = await fetch(`https://api.themoviedb.org/3/tv/top_rated?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=${currentPage}`);
			const raw_movie_data = await res.json();

			results = results.concat(raw_movie_data.results);	
  	}else if(currentData === "trending-shows"){
  		currentPage += 1;
			const res = await fetch(`https://api.themoviedb.org/3/trending/tv/week?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=${currentPage}`);
			const raw_movie_data = await res.json();

			results = results.concat(raw_movie_data.results);	
  	}
  };

	function slugToText(slug) {
	  var words = slug.split('-');

	  for (var i = 0; i < words.length; i++) {
	    var word = words[i];
	    words[i] = word.charAt(0).toUpperCase() + word.slice(1);
	  }

	  return words.join(' ');
	}
	function numberWithCommas(number) {
    	return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
	}

</script>

<svelte:head>
  <title>Tv Space - {slugToText(currentData)}</title>
</svelte:head>

<Title>
	{slugToText(currentData)}
</Title>


<main>
	<div class="cards">
		{#if results}
			{#each results as show}
	  			<Card 
	  				id="{show.id}" 
	  				name="{show.name}" 
	  				vote="{show.vote_average}"
	  				overview="{show.overview}" 
	  				language="{show.original_language}"
	  				posterPath="{show.poster_path}" 
	  			/>
	  		{/each}
		{:else}
			<div class="spinner">
	  			<Spinner />
			</div>
		{/if}
	</div>
	{#if currentPage < totalPage}
		<div class="btn-wrap">
	  		<button title="Page {currentPage} with {results.length} movies out of {numberWithCommas(total)} movies,Goto page {currentPage + 1}({totalPage})" class="more-btn" sveltekit:prefetch on:click={load_more}>show me more</button>
		</div>
	{/if}
</main>




<style>
	.spinner{
		margin: 5rem;
	}
	main{
	    padding: 0 2rem;
	    margin:0 auto;
	    margin-bottom: 1rem;
  }
  .cards{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 1rem;
  }
  .more-btn{
		display: flex;
		align-items:flex-end;
		text-align: center;
		outline: none;
		background-color: transparent;
		padding:0.4rem 0.8rem;
		border-radius: 2px;
		font-family: inherit;
		text-transform: capitalize;
		margin: 0.5rem auto;
		margin-bottom: 1.5rem;
		cursor: pointer;
		user-select: none;
		box-shadow: 0px 0px 0px 2px #ffdf00;
		color: #ffdf00;
		font-weight: 500;

	}
	.more-btn:hover{
		color: #0d1216;
		background-color: #ffdf00;
	}
</style>