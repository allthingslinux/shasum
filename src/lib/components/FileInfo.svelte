<script lang="ts">
  export let fileName: string;
  export let fileSize: number;
  export let progress: number;
  export let isProcessing: boolean;

  // Format file size for display
  const formatFileSize = (bytes: number) => {
    if (bytes === 0) return '0 Bytes';
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
  };
</script>

<div class="mt-6 w-full">
  <h3 class="mb-3 text-lg font-semibold text-zinc-300">File Information</h3>

  <div class="space-y-3 text-sm text-zinc-400">
    <div class="flex justify-between">
      <span>File Name:</span>
      <span class="max-w-xs text-right font-mono text-xs break-all text-zinc-300">
        {fileName || 'No file selected'}
      </span>
    </div>

    <div class="flex justify-between">
      <span>File Size:</span>
      <span class="text-zinc-300">{fileSize > 0 ? formatFileSize(fileSize) : 'N/A'}</span>
    </div>

    <div class="flex justify-between">
      <span>File Type:</span>
      <span class="text-zinc-300">
        {fileName ? fileName.split('.').pop()?.toUpperCase() || 'Unknown' : 'N/A'}
      </span>
    </div>

    {#if isProcessing}
      <div class="mt-4 rounded border border-zinc-700 bg-zinc-800 p-3">
        <div class="mb-2 flex items-center justify-between">
          <span class="font-medium text-zinc-300">Processing...</span>
          <span class="text-zinc-300">{progress}%</span>
        </div>
        <div class="h-2 w-full rounded-full bg-zinc-700">
          <div
            class="h-2 rounded-full bg-blue-600 transition-all duration-300"
            style="width: {progress}%"
          ></div>
        </div>
        <div class="mt-2 text-xs text-zinc-400">Generating MD5, SHA-256, and SHA-512 hashes...</div>
      </div>
    {:else}
      <div class="mt-4 rounded border border-zinc-700 bg-zinc-800 p-3">
        <div class="mb-2 flex items-center justify-between">
          <span class="font-medium text-zinc-300">Status:</span>
          <span class="text-zinc-300">{fileName ? 'Ready' : 'Waiting for file'}</span>
        </div>
        <div class="h-2 w-full rounded-full bg-zinc-700">
          <div class="h-2 w-full rounded-full bg-green-600"></div>
        </div>
        <div class="mt-2 text-xs text-zinc-400">
          {fileName ? 'Hash calculation complete' : 'Select a file to begin'}
        </div>
      </div>
    {/if}
  </div>

  <!-- Additional info -->
  <div class="mt-4 rounded border border-zinc-700 bg-zinc-800 p-3">
    <h4 class="mb-2 text-sm font-medium text-zinc-300">About Hash Functions</h4>
    <ul class="space-y-1 text-xs text-zinc-400">
      <li>• MD5: 128-bit hash (32 hex chars)</li>
      <li>• SHA-256: 256-bit hash (64 hex chars)</li>
      <li>• SHA-512: 512-bit hash (128 hex chars)</li>
    </ul>
  </div>
</div>
