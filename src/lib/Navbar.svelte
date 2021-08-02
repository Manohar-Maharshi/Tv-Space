<script>
	import Spinner from './Spinner.svelte';

	let val='';
	let timer;

	const debounce = v => {
		clearTimeout(timer);
		timer = setTimeout(() => {
			val = "";
			if(v && v.trim()){
				val = v;		
			}
		}, 750);
	}
	async function getTvShows(term) {
		let url = `https://api.themoviedb.org/3/search/tv?api_key=04c35731a5ee918f014970082a0088b1&language=en-US&page=1&query=${term}&include_adult=true`
		const shows_raw = await fetch(url);
		const shows_json = await shows_raw.json();

		return shows_json.results;
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
</script>
<nav>
	<a class="logo-text" href="/">
		Tv Space
	</a>
	<ul>
		<li class="search-container">
			<input on:keyup={({ target: { value } }) => debounce(value)}  type="search" placeholder="Search Tv Shows..">
			<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" fill="currentColor" viewBox="0 0 17 17">
  				<path stroke="currentColor" stroke-width="0.5" d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"/>
			</svg>
		</li>
		<!-- TODO
			<li>
				<button class="btn-more">
					<span>More</span>
					<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" fill="currentColor" viewBox="0 0 16 16">
					  <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM8.5 4.5a.5.5 0 0 0-1 0v5.793L5.354 8.146a.5.5 0 1 0-.708.708l3 3a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0-.708-.708L8.5 10.293V4.5z"/>
					</svg>
				</button>
			</li>
		-->

		{#if val}
			<section class="search-reasults-container">
				{#await getTvShows(val)}
					<Spinner />
				{:then showList}
					{#each showList as show}
						<div class="show-results">
							<a sveltekit:prefetch href={`/show/${slugify(show.original_name)}_${show.id}`} class="sample-movie-card">
			    						{#if show.poster_path === null}
					  						<img src="https://disc1.filmdownload.stream/assets/general/images/no_poster.jpg" alt="{show.original_name}">
										{:else}
											<img src="https://image.tmdb.org/t/p/w500{show.poster_path}" alt="{show.original_name}">
										{/if}
										<div class="meta-data">
												<p class="name">{show.original_name}</p>
												<p class="dates">
													<span>
														<svg xmlns="http://www.w3.org/2000/svg" width="9" height="9" fill="currentColor" class="bi bi-calendar2-check" viewBox="0 0 16 16">
														  <path d="M10.854 8.146a.5.5 0 0 1 0 .708l-3 3a.5.5 0 0 1-.708 0l-1.5-1.5a.5.5 0 0 1 .708-.708L7.5 10.793l2.646-2.647a.5.5 0 0 1 .708 0z"/>
														  <path d="M3.5 0a.5.5 0 0 1 .5.5V1h8V.5a.5.5 0 0 1 1 0V1h1a2 2 0 0 1 2 2v11a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V3a2 2 0 0 1 2-2h1V.5a.5.5 0 0 1 .5-.5zM2 2a1 1 0 0 0-1 1v11a1 1 0 0 0 1 1h12a1 1 0 0 0 1-1V3a1 1 0 0 0-1-1H2z"/>
														  <path d="M2.5 4a.5.5 0 0 1 .5-.5h10a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5H3a.5.5 0 0 1-.5-.5V4z"/>
														</svg>
														{formatDate(show.first_air_date)}
													</span>
													<span>&middot;</span>
													<span>
														<svg xmlns="http://www.w3.org/2000/svg" width="9" height="9" fill="currentColor" class="bi bi-translate" viewBox="0 0 16 16">
														  <path d="M4.545 6.714 4.11 8H3l1.862-5h1.284L8 8H6.833l-.435-1.286H4.545zm1.634-.736L5.5 3.956h-.049l-.679 2.022H6.18z"/>
														  <path d="M0 2a2 2 0 0 1 2-2h7a2 2 0 0 1 2 2v3h3a2 2 0 0 1 2 2v7a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2v-3H2a2 2 0 0 1-2-2V2zm2-1a1 1 0 0 0-1 1v7a1 1 0 0 0 1 1h7a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H2zm7.138 9.995c.193.301.402.583.63.846-.748.575-1.673 1.001-2.768 1.292.178.217.451.635.555.867 1.125-.359 2.08-.844 2.886-1.494.777.665 1.739 1.165 2.93 1.472.133-.254.414-.673.629-.89-1.125-.253-2.057-.694-2.82-1.284.681-.747 1.222-1.651 1.621-2.757H14V8h-3v1.047h.765c-.318.844-.74 1.546-1.272 2.13a6.066 6.066 0 0 1-.415-.492 1.988 1.988 0 0 1-.94.31z"/>
														</svg>
														{show.original_language.toUpperCase()}
													</span>
												</p>
										</div>
										<p class="vote">
											<span>
												{show.vote_average}
											</span>
										</p>
			    					</a>
						</div>
					{:else}
						<p class="seach-sug">Try Another...😵</p>
					{/each}
				{:catch}
					<p class="seach-sug">Try Another...😵</p>
				{/await}
			</section>
		{/if}
	</ul>
</nav>









<style>
	.search-reasults-container{
		z-index: 10;
		position: absolute;
		top: 4.5rem;
		right: 3rem;
		width: 22rem;
		height: 30rem;
		background-color: #161f26;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
		overflow-y: auto;
	}
	.seach-sug{
		width: 100%;
		height: 100%;
		margin: auto;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #DADADA;
		font-size: 1.4rem;
		font-weight: 500;
		text-transform: capitalize;
	}
	.sample-movie-card img{
		width: 3rem;
	}
	.sample-movie-card:hover .meta-data .name{
		color: #ffdf00;
	}
	.sample-movie-card{
		margin: 0.6rem auto;
		padding:0.3rem 0.5rem;
		display: flex;
		width: 95%;
	}
	.sample-movie-card:hover{
		background-color: #0d1216;
	}
	.meta-data{
		padding:0 0.5rem;
		padding-left: 0.7rem;
		display: flex;
		flex: 1;
		align-items: flex-start;
		flex-direction: column;
		line-height: 1.5;
	}
	.meta-data .name{
		line-height: 1.2;
		font-size: 1.1rem;
		color: #DADADA;
	}
	.meta-data .dates{
		opacity: 0.8;
		font-size: 0.8rem;
	}
	.vote{
		display: inline-block;
	}
	.vote span{
		padding: 0 0.4rem;
		font-size: 0.9rem;
		border: 3px;
		color: #0d1216;
		background-color: #ffdf00;

	}
	nav{
		position: relative;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 1.1rem 3rem;
		background-color: #0d1216;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
	}
	.logo-text{
		font-size: 1.1rem;
		font-weight: 400;
		box-shadow: 0px 0px 0px 2px #ffdf00;
		padding:0.15rem 0.7rem;
		text-align: center;
		color: #ffdf00;
		border-radius: 1px;
		display: flex;
		align-items: center;
		transition: 0.1s all ease-in-out;
	}
	.logo-text:hover,.logo-text:focus{
		box-shadow: 0px 0px 0px 3px #192129;

	}
	ul{
		display: flex;
		align-items: center;
	}
	.search-container{
		display: flex;
		align-items: center;
		flex-direction: row-reverse;
	}
	.search-container svg{
		position: relative;
		left: 1.8rem;
		fill: #DADADA;
		opacity: 0.5;
	}
	input[type="search"]{
		width: 22rem;
		background-color: #192129;
		font-size: 1rem;
		padding:0.45rem 1rem;
		padding-left: 2.5rem;
		cursor: auto;
		caret-color: #DADADA;
		border-radius: 2px;
		transition: 0.3s box-shadow,fill;
	}
	input[type="search"]:focus + svg{
		fill: #ffdf00;
		opacity: 1;
	}
	.btn-more{
		margin-left: 1rem;
		padding:0.45rem 1rem;
		background-color: #192129;
		display: flex;
		align-items: center;
		color: #c4cfd6;
	}
	.btn-more span{
		margin-right: 0.5rem;
	}
	.btn-more:hover span,.btn-more:hover svg{
		color: #ffdf00;
	}
</style>