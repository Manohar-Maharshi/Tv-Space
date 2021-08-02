<script>
	export let show = [];



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


<div class="show-info">
	<div class="img-container">
		<img src="https://image.tmdb.org/t/p/w500{show.poster_path}" alt="{show.name}">
		<div class="show-stats">
			<p class="popularity-stat">
				<span title="Based on Number of Votes for the day,Views for the day,Number of total votes,Next/last episode to air date,Previous days score" class="stat-name">Popularity</span>
				<span class="stat-no">{show.popularity}</span>
			</p>
		</div>
	</div>
	<div class="details-container">
		<div class="main-details">
			<p class="main-tagline"><span class="main-subheading">Name: </span>{show.name}</p>
			{#if show.tagline}
				<p class="main-tagline"><span class="main-subheading">Tagline: </span>{show.tagline}</p>
			{/if}
			{#if show.first_air_date}
				<p class="main-tagline"><span class="main-subheading">First Air Date: </span>{formatDate(show.first_air_date)}</p>		
			{/if}
			{#if show.last_air_date}
				<p class="main-tagline"><span class="main-subheading">Last Air Date: </span>{formatDate(show.last_air_date)}</p>		
			{/if}
			{#if show.created_by}
				<p class="main-tagline"><span class="main-subheading">Created By: </span>
					{#each show.created_by.splice(0,1) as creator}
						{creator.name}
					{:else}
						<span class="error">No Details</span>
					{/each}
				</p>				
			{/if}
			{#if show.number_of_seasons}
				<p class="main-tagline"><span class="main-subheading">No. Seasons: </span>{show.number_of_seasons}</p>		
			{/if}
			{#if show.number_of_episodes}
				<p class="main-tagline"><span class="main-subheading">No. Episodes: </span>{show.number_of_episodes}</p>		
			{/if}
			{#if show.episode_run_time}
				<p class="main-tagline">
					<span class="main-subheading">Min. Episode Runtime: </span>
					{show.episode_run_time[0]} Min
				</p>		
			{/if}
			{#if show.original_name}
				<p class="main-tagline"><span class="main-subheading">Original Name: </span>{show.original_name}</p>
			{/if}
			{#if show.spoken_languages}
				<p class="main-tagline">
					<span class="main-subheading">Original Language: </span>
					{#each show.spoken_languages as lang}
						{lang.english_name}
					{/each}
				</p>
			{/if}
			{#if show.production_companies > 0}
				<p class="main-tagline">
					<span class="main-subheading">Production Companies: </span>
					{show.production_companies[0].name}({show.production_companies[0].origin_country})
				</p>
			{:else}
				<p class="main-tagline">
					<span class="main-subheading">Production Companies: </span>
						<span class="error">No Details</span>
					</p>
			{/if}
			{#if show.homepage}
				<p class="main-tagline homepage">
					<span class="main-subheading">Homepage: </span>
					<a target="_blank" href="{show.homepage}">{show.homepage}</a>
				</p>
			{/if}
		</div>
	</div>
	<div class="follow-container">
		<div class="follow-flex">
			{#if show.external_ids}
					{#if show.external_ids.imdb_id}
						<a target="_blank" href="https://www.imdb.com/title/{show.external_ids.imdb_id}">
							<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 24 24">
								<title>IMDb</title>
								<path d="M22.3781 0H1.6218C.7411.0583.0587.7437.0018 1.5953l-.001 20.783c.0585.8761.7125 1.543 1.5559 1.6191A.337.337 0 0 0 1.6016 24h20.7971a.4579.4579 0 0 0 .0437-.002c.8727-.0768 1.5568-.8271 1.5568-1.7085V1.7098c0-.8914-.696-1.6416-1.584-1.7078A.3294.3294 0 0 0 22.3781 0zm0 .496a1.2144 1.2144 0 0 1 1.1252 1.2139v20.5797c0 .6377-.4875 1.1602-1.1045 1.2145H1.6016c-.5967-.0543-1.0645-.5297-1.1053-1.1258V1.6284C.5371 1.0185 1.0184.5364 1.6217.496h20.7564zM4.7954 8.2603v7.3636H2.8899V8.2603h1.9055zm6.5367 0v7.3636H9.6707v-4.9704l-.6711 4.9704H7.813l-.6986-4.8618-.0066 4.8618h-1.668V8.2603h2.468c.0748.4476.1492.9694.2307 1.5734l.2712 1.8713.4407-3.4447h2.4817zm2.9772 1.3289c.0742.0404.122.108.1417.2034.0279.0953.0345.3118.0345.6442v2.8548c0 .4881-.0345.7867-.0955.8954-.0609.1152-.2304.1695-.5018.1695V9.5211c.204 0 .3457.0205.4211.0681zm-.0211 6.0347c.4543 0 .8006-.0265 1.0245-.0742.2304-.0477.4204-.1357.5694-.2648.1556-.1218.2642-.298.3251-.5219.0611-.2238.1021-.6648.1021-1.3224v-2.5832c0-.6986-.0271-1.1668-.0742-1.4039-.041-.237-.1431-.4543-.3126-.6437-.1695-.1973-.4198-.3324-.7456-.421-.3191-.0808-.8542-.1285-1.7694-.1285h-1.4244v7.3636h2.3051zm5.14-1.7827c0 .3523-.0199.5762-.0544.6708-.033.0947-.1894.1424-.3046.1424-.1086 0-.19-.0477-.2238-.1351-.041-.0887-.0609-.2986-.0609-.6238v-1.9469c0-.3324.0199-.5423.0543-.6237.0338-.0808.1086-.122.2171-.122.1153 0 .2709.0412.3114.1425.041.0947.0609.2986.0609.6032v1.8926zm-2.4747-5.5809v7.3636h1.7157l.1152-.4675c.1556.1894.3251.3324.5152.4271.1828.0881.4608.1357.678.1357.3047 0 .5629-.0748.7802-.237.2165-.1562.3589-.3462.4198-.5628.0543-.2173.0887-.543.0887-.9841v-2.0675c0-.4409-.0139-.7324-.0344-.8681-.0199-.1357-.0742-.2781-.1695-.4204-.1021-.1425-.2437-.251-.4272-.3325-.1834-.0742-.3999-.1152-.6576-.1152-.2172 0-.4952.0477-.6846.1285-.1835.0887-.353.2238-.5086.4007V8.2603h-1.8309z"/>
							</svg>
						</a>
					{/if}
					{#if show.external_ids.facebook_id}
						<a target="_blank" href="http://www.facebook.com/{show.external_ids.facebook_id}">
							<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-facebook" viewBox="0 0 16 16">
							  <path d="M16 8.049c0-4.446-3.582-8.05-8-8.05C3.58 0-.002 3.603-.002 8.05c0 4.017 2.926 7.347 6.75 7.951v-5.625h-2.03V8.05H6.75V6.275c0-2.017 1.195-3.131 3.022-3.131.876 0 1.791.157 1.791.157v1.98h-1.009c-.993 0-1.303.621-1.303 1.258v1.51h2.218l-.354 2.326H9.25V16c3.824-.604 6.75-3.934 6.75-7.951z"/>
							</svg>
						</a>
					{/if}
					{#if show.external_ids.instagram_id}
						<a target="_blank" href="http://www.instagram.com/{show.external_ids.instagram_id}">
							<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-instagram" viewBox="0 0 16 16">
	  									<path d="M8 0C5.829 0 5.556.01 4.703.048 3.85.088 3.269.222 2.76.42a3.917 3.917 0 0 0-1.417.923A3.927 3.927 0 0 0 .42 2.76C.222 3.268.087 3.85.048 4.7.01 5.555 0 5.827 0 8.001c0 2.172.01 2.444.048 3.297.04.852.174 1.433.372 1.942.205.526.478.972.923 1.417.444.445.89.719 1.416.923.51.198 1.09.333 1.942.372C5.555 15.99 5.827 16 8 16s2.444-.01 3.298-.048c.851-.04 1.434-.174 1.943-.372a3.916 3.916 0 0 0 1.416-.923c.445-.445.718-.891.923-1.417.197-.509.332-1.09.372-1.942C15.99 10.445 16 10.173 16 8s-.01-2.445-.048-3.299c-.04-.851-.175-1.433-.372-1.941a3.926 3.926 0 0 0-.923-1.417A3.911 3.911 0 0 0 13.24.42c-.51-.198-1.092-.333-1.943-.372C10.443.01 10.172 0 7.998 0h.003zm-.717 1.442h.718c2.136 0 2.389.007 3.232.046.78.035 1.204.166 1.486.275.373.145.64.319.92.599.28.28.453.546.598.92.11.281.24.705.275 1.485.039.843.047 1.096.047 3.231s-.008 2.389-.047 3.232c-.035.78-.166 1.203-.275 1.485a2.47 2.47 0 0 1-.599.919c-.28.28-.546.453-.92.598-.28.11-.704.24-1.485.276-.843.038-1.096.047-3.232.047s-2.39-.009-3.233-.047c-.78-.036-1.203-.166-1.485-.276a2.478 2.478 0 0 1-.92-.598 2.48 2.48 0 0 1-.6-.92c-.109-.281-.24-.705-.275-1.485-.038-.843-.046-1.096-.046-3.233 0-2.136.008-2.388.046-3.231.036-.78.166-1.204.276-1.486.145-.373.319-.64.599-.92.28-.28.546-.453.92-.598.282-.11.705-.24 1.485-.276.738-.034 1.024-.044 2.515-.045v.002zm4.988 1.328a.96.96 0 1 0 0 1.92.96.96 0 0 0 0-1.92zm-4.27 1.122a4.109 4.109 0 1 0 0 8.217 4.109 4.109 0 0 0 0-8.217zm0 1.441a2.667 2.667 0 1 1 0 5.334 2.667 2.667 0 0 1 0-5.334z"/>
							</svg>
						</a>							
					{/if}
					{#if show.external_ids.twitter_id}
						<a target="_blank" href="http://twitter.com/{show.external_ids.twitter_id}">
							<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-twitter" viewBox="0 0 16 16">
							  <path d="M5.026 15c6.038 0 9.341-5.003 9.341-9.334 0-.14 0-.282-.006-.422A6.685 6.685 0 0 0 16 3.542a6.658 6.658 0 0 1-1.889.518 3.301 3.301 0 0 0 1.447-1.817 6.533 6.533 0 0 1-2.087.793A3.286 3.286 0 0 0 7.875 6.03a9.325 9.325 0 0 1-6.767-3.429 3.289 3.289 0 0 0 1.018 4.382A3.323 3.323 0 0 1 .64 6.575v.045a3.288 3.288 0 0 0 2.632 3.218 3.203 3.203 0 0 1-.865.115 3.23 3.23 0 0 1-.614-.057 3.283 3.283 0 0 0 3.067 2.277A6.588 6.588 0 0 1 .78 13.58a6.32 6.32 0 0 1-.78-.045A9.344 9.344 0 0 0 5.026 15z"/>
							</svg>
						</a>							
					{/if}
					<a target="_blank" href="https://www.themoviedb.org/tv/{show.id}">
						<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"  viewBox="0 0 24 24">
							<title>The Movie Database</title>
							<path d="M19.491 21.899c2.106 0 3.531-1.424 3.531-3.531V3.531C23.022 1.425 21.598 0 19.491 0H4.509C2.403 0 .978 1.424.978 3.531V24l1.809-2.101V3.531a1.721 1.721 0 0 1 1.719-1.719h14.982c.949.002 1.718.77 1.719 1.719v14.837a1.721 1.721 0 0 1-1.719 1.719H6.92l-1.81 1.812-.011-.014zM8.787 11.466H7.09v5.698h1.697c3.793 0 3.793-5.698 0-5.698zm0 4.559h-.551v-3.419h.551c2.215 0 2.215 3.418 0 3.418zM8.456 10.389h1.139V5.83h1.418V4.699H7.037V5.83h1.419v4.559zM14.063 7.201l-1.971-2.502h-.366v5.785h1.156v-3.18l1.182 1.531 1.183-1.531-.008 3.18h1.156V4.699h-.36l-1.971 2.502zM15.983 14.315c.358-.247.51-.689.526-1.124.023-1.004-.606-1.729-1.617-1.729h-2.255v5.706h2.255a1.695 1.695 0 0 0 1.681-1.694v-.02-.008c0-.466-.231-.878-.585-1.127l-.004-.003zm-2.204-1.714h1.013c.327 0 .526.255.526.573a.533.533 0 0 1-.526.574h-1.013V12.6zm1.013 3.427h-1.013v-1.139h1.027c.309 0 .559.25.559.559v.014a.566.566 0 0 1-.566.566h-.001z"/>
						</svg>
					</a>							
			{/if}
		</div>
	</div>
</div>



<style>
	.error{
		font-style: oblique;
		font-size: 1rem;
	}
	.homepage a{
		font-size: 1rem;
	}
	.homepage a:hover{
		color: #ffe219;
		text-decoration: underline;
	}
	.follow-container{
		flex: 1;
	}
	.follow-flex{
		width: 100%;
		height: 100%;
		display: inline-flex;
		align-items: center;
		flex-direction: column;
		justify-content: center;
	}
	.follow-flex a{
		margin-bottom: 1.8rem;
	}
	.follow-flex a svg{
		width: 2.5rem;
		height: 2.5rem;
		opacity: 0.6;
	}
	.follow-flex a svg:hover{
		fill: #ffdf00;
		opacity: 1;
	}
	.details-container{
		height: 100%;
		/*padding: 1rem 0;*/
		display: inline-flex;
		align-items: flex-start;
		justify-content: flex-start;
	}
	.show-name{
		text-align: left;
		font-size: 2rem;
		font-weight: 500;
		color: #ffdf00;
		margin-left: 2rem;
	}
	.main-details{
		line-height: 2;
	}
	.main-tagline{
		text-transform: capitalize;
		font-size: 1.1rem;
	}
	.main-subheading{
		text-transform: capitalize;
		margin-right: 0.2rem;
		font-weight: 500;
		color: #ffdf00;
	}
	.show-info{
		width: 90%;
		height: 29rem;
		background-color: #192129;
		margin: 1rem auto;
		display: flex;
	}
	.img-container{
		flex: 1;
		display: inline-flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}
	.show-stats{
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		margin-top: 1rem;
	}
	.show-stats p{
		cursor: default;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: center;
	}
	.stat-name{
		background-color: #ffdf00;
		color: #192129;
		font-weight: bold;
		padding: 0.1rem 1rem;
		text-transform: uppercase;
		font-size: 1.15rem;
	}
	.stat-no{
		padding-top: 0.1rem;
		font-size: 1.8rem;
		font-weight: 500;
	}
	.details-container{
		padding: 1rem 0;
		flex: 2;
		display: flex;
		align-items: flex-start;
		justify-content: flex-start;
		flex-direction: column;
		height: 100%;
		width: 100%;
	}
	.show-info img{
		width: 13rem;
  		object-fit: cover;
    	padding: 0.3rem;
		box-shadow: 0px 0px 0px 2px #ffdf00;
	}
</style>