<script>
	import Play from '$lib/PlayButton.svelte'

	export let posterPath = "";
	export let name = "";
	export let overview = "";
	export let id ="";
	export let vote = "";
	export let language = "1";

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

<a sveltekit:prefetch href={`/show/${slugify(name)}_${id}`} title="{name}" class="card-container">
	<div class="card">
		{#if posterPath}
			<img class="card-img" src="https://image.tmdb.org/t/p/w500{posterPath}" alt="{name}">
		{:else}
			<span class="fall-back-text">{name}</span>
		{/if}
	</div>
	<span class="card-tag">
		<p class="vote-count">{vote} &bull; {language.toUpperCase()}</p>
	</span>
	<div class="card-body">
		<Play />
	</div>
</a>



<style>
	.card-container{
		margin: 0.5rem;
		background-color: #0d1216;
		box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
		position: relative;
	}
  .card{
  	width: 13rem;
  	height: 18rem;
  }
  .card-img{
  	width: 100%;
  	height: 100%;
  	object-fit: cover;
  	border-radius: 5px;
  }
  .card-tag{
  	display: inline-block;	
  	position: absolute;
  	top: 0.5rem;
  }
  .vote-count{
  	text-align: center;
  	color: #0d1216;
  	padding: 0 0.7rem;
  	font-size: 0.9rem;
  	background-color: #ffdf00;
  	font-weight: 500;
  }
  .card-body{
  	opacity: 0;
  	position:absolute;
  	bottom: 0;
  	top: 0;
  	left: 0;
  	right: 0;
  	width: 100%;
  	height: 100%;
  	border-radius: 5px;
  	background-color: rgba(0, 0, 0, 0.5);
		box-shadow: 0px 0px 0px 2px #ffdf00;
  	display: flex;
  	align-items: center;
  	justify-content: center;
  	transition: .2s ease;
  }
	.fall-back-text{
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: bold;
		text-align: center;
		font-size: 1.3rem;
	}
	.card-container:hover .card-body{
		opacity: 1;
	}
</style>