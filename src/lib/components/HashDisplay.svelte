<script lang="ts">
  export let label: string;
  export let hash: string;
  export let loading: boolean;
  export let onCopy: (text: string) => void;

  let copied = false;
</script>

<div>
  <div class="mb-2 flex items-center justify-between">
    <span class="text-sm font-medium text-zinc-300">{label}</span>
    <button
      class="rounded bg-zinc-700 px-2 py-1 text-xs transition-colors hover:bg-zinc-600 disabled:opacity-50"
      disabled={!hash}
      on:click={() => (onCopy(hash), (copied = true), setTimeout(() => (copied = false), 2000))}
    >
      {copied ? 'Copied!' : 'Copy Full Hash'}
    </button>
  </div>
  <div class="relative">
    {#if loading}
      <div class="flex h-10 items-center justify-center rounded border border-zinc-700 bg-zinc-800">
        <span class="icon-[material-symbols--hourglass-top] animate-spin"></span>
      </div>
    {:else}
      <div class="flex h-10 items-center rounded border border-zinc-700 bg-zinc-950 px-3">
        <code class="text-s truncate text-center font-mono text-green-400">
          {hash || `Select a file to generate ${label} hash`}
        </code>
      </div>
    {/if}
  </div>
</div>
