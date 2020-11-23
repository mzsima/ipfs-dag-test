<script>
	import ipfsClient from 'ipfs-http-client'
	import { onMount } from 'svelte';

	export let text;
	let cid1;
	let query;
	let queryResult;

	let ipfs;
	onMount(() => {
		ipfs = ipfsClient('/ip4/127.0.0.1/tcp/5001')
	})

	async function dagPut() {
		cid1 = await ipfs.dag.put(JSON.parse(text))
	}
	async function dagGet() {
		queryResult = (await ipfs.dag.get(query)).value
	}
</script>

<main>
	<h1>IPFS DAG!</h1>
	
	<textarea bind:value={text}></textarea>
	<button on:click={dagPut} >DAG PUT</button>
	<div>CID: {cid1}</div>
	<hr/>
	<input bind:value={query}/>
	<button on:click={dagGet} >DAG Get</button>
	<pre>{JSON.stringify(queryResult, null, 2)}</pre>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 440px;
		margin: 0 auto;
	}
	pre { width: 100%; text-align: left; }
	input { width: 100%; }
	textarea { width: 100%; height: 100px; }
</style>