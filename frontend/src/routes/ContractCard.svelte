<script lang="ts">
  export let contract;
</script>

<div class="p-4 border rounded-md shadow-lg">
  <h3 class="text-lg font-semibold">{contract.title}</h3>
  <p><strong>Address:</strong> {contract.address}</p>
  <p><strong>ABI:</strong> {contract.abi}</p>
</div>

