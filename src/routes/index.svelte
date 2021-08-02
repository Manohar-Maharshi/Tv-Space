<script>
	import Card from '$lib/Card.svelte';
	import Spinner from '$lib/Spinner.svelte';
	import {slide} from 'svelte/transition'


	let showNowShows = false;
	let showTodayShow = false;
	let showPopularShow = false;
	let showTopRatedShow = false;
	let showTrendingShow = false;


 	function slugify(string) {
		return string
			.toString()
			.trim()
			.toLowerCase()
			.replace(/\s+/g, "-")
			.replace(/[^\w\-]+/g, "")
			.replace(/\-\-+/g, "-")
			.replace(/^-+/, "")
			.replace(/-+$/, "");
	}
	async function nowPlaying() {
		const todayShows_raw = await fetch(`https://api.themoviedb.org/3/tv/on_the_air?api_key=04c35731a5ee918f014970082a0088b1&language=en-US`);
		const todayShows_json = await todayShows_raw.json();

		return todayShows_json.results;
	}
	async function todayPlaying() {
		const todayShows_raw = await fetch(`https://api.themoviedb.org/3/tv/airing_today?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1`);
		const todayShows_json = await todayShows_raw.json();

		return todayShows_json.results;
	}
	async function popularPlaying() {
		const todayShows_raw = await fetch(`https://api.themoviedb.org/3/tv/popular?api_key=04c35731a5ee918f014970082a0088b1&language=en-US`);
		const todayShows_json = await todayShows_raw.json();

		return todayShows_json.results;
	}
	async function topRatedPlaying() {
		const todayShows_raw = await fetch(`https://api.themoviedb.org/3/tv/top_rated?api_key=04c35731a5ee918f014970082a0088b1&language=en-US`);
		const todayShows_json = await todayShows_raw.json();

		return todayShows_json.results;
	}
	async function trendingPlaying() {
		const todayShows_raw = await fetch(`https://api.themoviedb.org/3/trending/tv/week?api_key=04c35731a5ee918f014970082a0088b1`);
		const todayShows_json = await todayShows_raw.json();

		return todayShows_json.results;
	}
</script>


<svelte:head>
  <title>Tv Space - Home</title>
</svelte:head>



<main >
  <section class="heading">
    <h2>Now Streming</h2>
    <a href="/now-streaming">
    	<span>Explore All</span>
		<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-right" viewBox="0 0 16 16">
		  <path fill-rule="evenodd" d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"/>
		</svg>
    </a>
  </section>
  <section class="cards" >
	{#await nowPlaying()}
	  	<Spinner />
	{:then nowPlayingShows}
		{#if showNowShows}
			{#each nowPlayingShows.slice(0,20) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{:else}
	  		{#each nowPlayingShows.slice(0,5) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{/if}
	{:catch}
		<a  href="javascript:location.reload(true)" class="error">
			Refresh
			<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#ffdf00" class="bi bi-arrow-repeat" viewBox="0 0 16 16">
  				<path d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"/>
  				<path fill-rule="evenodd" d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"/>
			</svg>
		</a>
	{/await}
  </section>
  <section  class="load-more">
  	<button on:click={() => showNowShows ? showNowShows=false : showNowShows=true} >
  		{#if showNowShows}
  			Show Less
  		{:else}
  			Show More
  		{/if}
  	</button>
  </section>
</main>

<main >
  <section class="heading">
    <h2>Trending Shows</h2>
    <a href="/trending-shows">
    	<span>Explore All</span>
		<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-right" viewBox="0 0 16 16">
		  <path fill-rule="evenodd" d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"/>
		</svg>
    </a>
  </section>
  <section class="cards" >
	{#await trendingPlaying()}
	  	<Spinner />
	{:then trendingShows}
		{#if showTrendingShow}
			{#each trendingShows.slice(0,20) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{:else}
	  		{#each trendingShows.slice(0,5) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{/if}
	{:catch}
		<a  href="javascript:location.reload(true)" class="error">
			Refresh
			<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#ffdf00" class="bi bi-arrow-repeat" viewBox="0 0 16 16">
  				<path d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"/>
  				<path fill-rule="evenodd" d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"/>
			</svg>
		</a>
	{/await}
  </section>
  <section  class="load-more">
  	<button on:click={() => showTrendingShow ? showTrendingShow=false : showTrendingShow=true} >
  		{#if showTrendingShow}
  			Show Less
  		{:else}
  			Show More
  		{/if}
  	</button>
  </section>
</main>

<main >
  <section class="heading">
    <h2>Today Shows</h2>
    <a href="/today-shows">
    	<span>Explore All</span>
		<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-right" viewBox="0 0 16 16">
		  <path fill-rule="evenodd" d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"/>
		</svg>
    </a>
  </section>
  <section class="cards" >
	{#await todayPlaying()}
	  	<Spinner />
	{:then todayPlayingShows}
		{#if showTodayShow}
			{#each todayPlayingShows.slice(0,20) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{:else}
	  		{#each todayPlayingShows.slice(0,5) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{/if}
	{:catch}
		<a  href="javascript:location.reload(true)" class="error">
			Refresh
			<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#ffdf00" class="bi bi-arrow-repeat" viewBox="0 0 16 16">
  				<path d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"/>
  				<path fill-rule="evenodd" d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"/>
			</svg>
		</a>
	{/await}

  </section>
  <section  class="load-more">
  	<button on:click={() => showTodayShow ? showTodayShow=false : showTodayShow=true} >
  		{#if showTodayShow}
  			Show Less
  		{:else}
  			Show More
  		{/if}
  	</button>
  </section>
</main>

<main >
  <section class="heading">
    <h2>Popular Shows</h2>
    <a href="/popular-shows">
    	<span>Explore All</span>
		<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-right" viewBox="0 0 16 16">
		  <path fill-rule="evenodd" d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"/>
		</svg>
    </a>
  </section>
  <section class="cards" >
	{#await popularPlaying()}
	  	<Spinner />
	{:then todayPlayingShows}
		{#if showPopularShow}
			{#each todayPlayingShows.slice(0,20) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{:else}
	  		{#each todayPlayingShows.slice(0,5) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{/if}
	{:catch}
		<a  href="javascript:location.reload(true)" class="error">
			Refresh
			<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#ffdf00" class="bi bi-arrow-repeat" viewBox="0 0 16 16">
  				<path d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"/>
  				<path fill-rule="evenodd" d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"/>
			</svg>
		</a>
	{/await}
  </section>
  <section  class="load-more">
  	<button on:click={() => showPopularShow ? showPopularShow=false : showPopularShow=true} >
  		{#if showPopularShow}
  			Show Less
  		{:else}
  			Show More
  		{/if}
  	</button>
  </section>
</main>

<main >
  <section class="heading">
    <h2>Top Rated</h2>
    <a href="/top-rated">
    	<span>Explore All</span>
		<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-chevron-right" viewBox="0 0 16 16">
		  <path fill-rule="evenodd" d="M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z"/>
		</svg>
    </a>
  </section>
  <section class="cards" >
	{#await topRatedPlaying()}
	  	<Spinner />
	{:then todayPlayingShows}
		{#if showTopRatedShow}
			{#each todayPlayingShows.slice(0,20) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{:else}
	  		{#each todayPlayingShows.slice(0,5) as show}
	  			<Card vote={show.vote_average} language="{show.original_language}" id={show.id} overview="{show.overview}" posterPath="{show.poster_path}" name="{show.name}" />
	  		{/each}
	  	{/if}
	{:catch}
		<a  href="javascript:location.reload(true)" class="error">
			Refresh
			<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#ffdf00" class="bi bi-arrow-repeat" viewBox="0 0 16 16">
  				<path d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"/>
  				<path fill-rule="evenodd" d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"/>
			</svg>
		</a>
	{/await}
  </section>
  <section  class="load-more">
  	<button on:click={() => showTopRatedShow ? showTopRatedShow=false : showTopRatedShow=true} >
  		{#if showTopRatedShow}
  			Show Less
  		{:else}
  			Show More
  		{/if}
  	</button>
  </section>
</main>

<style>
	.error{
		display: inline-flex;
		align-items: center;
		justify-content: center;
		color: #ffdf00;
		flex-direction: column;
		height: 18rem;
		font-size: 1.1rem;
		cursor: pointer;
	}
	.error svg{
		width: 3rem;
		height: 3rem;
	}
  main{
    width: 90%;
    margin:2rem auto;
    margin-bottom: 1rem;
  }
  .cards{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 1rem;
  }
  .heading{
    display: flex;
    align-items: center;
    justify-content: space-between;
    box-shadow: 0px 3px 0px 0px #ffdf00;
    margin: 1rem 0;
    user-select: none;
  }
  .heading h2{
    font-size: 1.2rem;
    text-transform: capitalize;
    background-color: #ffdf00;
    color: #0d1216;
    font-weight: 500;
    padding: 0.2rem 0.5rem;
  }
  .heading a{
    display: inline-flex;
    align-items: center;
    color: #c4cfd6;
    font-weight: 500;
  }
  .heading span{
    margin-right: 0.2rem;
  }
  .heading a:hover{
  	color: #ffdf00;
  }
  .load-more{
  	display:flex;
  	align-items: center;
  	justify-content: flex-end;
  	color: #ffdf00;
  	margin-top: -0.3rem;
  }
  .load-more button{
  	text-align: center;
	box-shadow: 0px 0px 0px 2px #ffdf00;
  	font-size: 1rem;
  	padding:0.3rem 0.8rem;
  }
  button:hover{
  	background-color: #ffdf00;
  	color: #0d1216;
  }
</style>