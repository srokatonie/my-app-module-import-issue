<script lang="ts">
  import { onMount } from "svelte";
  import { Buffer } from "buffer";

  let hashconnect: any
	let pairingString: string
	let connectionMessage: string | null
	let connected = false
	let tokenSymbol = 'TTT1';

  onMount(async () => {
    global.Buffer = Buffer;

    const { HashConnect } = await import("hashconnect");

    hashconnect = new HashConnect()

    let appMetadata = {
      name: "dApp Example",
      description: "An example hedera dApp",
      icon: "/svelte-welcome.png",
    }

		registerEvents()

    let initData = await hashconnect.init(appMetadata, "testnet")
		console.log({initData})
		const state = await hashconnect.connect()
		console.log({state})
		pairingString = hashconnect.generatePairingString(state, "testnet", false)
		hashconnect.findLocalWallets()
		if (pairingString) {
			connected = true
		}
  })

	function registerEvents() {
		hashconnect.foundExtensionEvent.once((walletMetadata: any) => {
			console.log({walletMetadata})
			hashconnect.connectToLocalWallet(pairingString)
		})
	
		hashconnect.pairingEvent.once((pairingData: any) => {
			console.log({pairingData})
			connected = true
			connectionMessage = null
		})
	
		hashconnect.acknowledgeMessageEvent.once((acknowledgeData: any) => {
			console.log({acknowledgeData})
		})
	}
</script>

<h1>
	Testing HashConnect
</h1>