<script lang="ts">
	
import { onMount } from 'svelte'

let hashconnect
onMount(async () => {
	const { HashConnect } = await import('hashconnect')

	hashconnect = new HashConnect(true)

	let appMetadata = {
		name: "dApp Example",
		description: "An example hedera dApp",
		icon: "/svelte-welcome.png"
	}

	// This fails:
	let initData = await hashconnect.init(appMetadata, "testnet");

})

</script>

<h1>
	Testing HashConnect
</h1>