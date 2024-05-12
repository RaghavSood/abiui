<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<script lang="ts">
  import { writable } from 'svelte/store';
  import ContractCard from './ContractCard.svelte';
  import { onMount } from 'svelte'
  import { defaultEvmStores as evm } from 'svelte-web3'
  import { web3, selectedAccount, connected, chainId } from 'svelte-web3'

  let address = '';
  let abi = '';
  let title = '';
  let contracts = writable([]);

  function addContract() {
    contracts.update(currentContracts => [
      ...currentContracts,
      { title, address, abi }
    ]);

    // Clear the form fields after adding the contract
    address = '';
    abi = '';
    title = '';
  }

  onMount(() => {
  })

</script>

<main>
  {#if $connected}
    <p>Connected to {$chainId} with account {$selectedAccount}</p>
  {:else}
    <button on:click={() => evm.setProvider()} class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-500 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">Connect Wallet</button>
  {/if}
  <form on:submit|preventDefault={addContract} class="space-y-4">
  <div>
    <label for="title" class="block text-sm font-medium text-gray-700">Title</label>
    <input type="text" id="title" bind:value={title} class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" placeholder="Contract Title" required>
  </div>
  <div>
    <label for="address" class="block text-sm font-medium text-gray-700">Address</label>
    <input type="text" id="address" bind:value={address} class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" placeholder="Contract Address" required>
  </div>
  <div>
    <label for="abi" class="block text-sm font-medium text-gray-700">ABI</label>
    <textarea id="abi" bind:value={abi} rows="3" class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" placeholder="Contract ABI" required></textarea>
  </div>
  <button type="submit" class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-500 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">Add Contract</button>
</form>

{#each $contracts as contract (contract.address)}
  <ContractCard {contract} />
{/each}
</main>

