<script lang="ts">
  export let fileName: string;
  export let fileSize: number;
  export let progress: number;
  export let isProcessing: boolean;
  export let eta: string;
  export let statusLog: string;
  export let processingTime: string;
  export let onFileChange: (event: Event) => void;

  // Format file size for display
  const formatFileSize = (bytes: number) => {
    if (bytes === 0) return '0 Bytes';
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
  };
</script>

<div class="flex flex-col items-center rounded-lg bg-zinc-900 p-6 shadow-lg">
  <h1 class="mb-4 text-center text-2xl font-bold">Please upload your file</h1>
  <div
    class="relative flex items-center overflow-hidden rounded border border-zinc-700 bg-zinc-800"
  >
    <label class="m-0 flex w-full cursor-pointer items-center">
      <input
        type="file"
        accept=".iso"
        class="absolute inset-0 h-full w-full opacity-0"
        on:change={onFileChange}
      />
      <span class="bg-zinc-700 px-3 py-2 text-sm text-white">Browse...</span>
      <span class="flex-1 px-3 py-2 text-sm text-zinc-400">
        {fileName || 'No file selected.'}
      </span>
    </label>
  </div>

  <!-- File info and progress section -->
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
        <span>Processing Time:</span>
        <span class="font-medium text-green-400">{processingTime || 'N/A'}</span>
      </div>

      {#if isProcessing}
        <div class="mt-4 rounded border border-zinc-700 bg-zinc-800 p-3">
          <div class="mb-2 flex items-center justify-between">
            <span class="font-medium text-zinc-300">Status:</span>
            <div class="text-right">
              <div class="text-zinc-300">
                <span class="text-zinc-500">{eta ? `ETA: ${eta} ` : ''}</span>{progress}%
              </div>
            </div>
          </div>
          <div class="h-2 w-full rounded-full bg-zinc-700">
            <div
              class="h-2 rounded-full bg-blue-600 transition-all duration-300"
              style="width: {progress}%"
            ></div>
          </div>
          <div class="mt-2 text-xs text-zinc-400">
            {statusLog}
          </div>
        </div>
      {:else}
        <div class="mt-4 rounded border border-zinc-700 bg-zinc-800 p-3">
          <div class="mb-2 flex items-center justify-between">
            <span class="font-medium text-zinc-300">Status:</span>
            <span class="text-zinc-300">Ready</span>
          </div>
          <div class="h-2 w-full rounded-full bg-zinc-700">
            <div class="h-2 w-full rounded-full bg-green-600"></div>
          </div>
          <div class="mt-2 text-xs text-zinc-400">
            {fileName ? 'Hash calculation complete' : 'Select a file to begin'}
            {#if processingTime && fileName}
              <br />
              <span class="text-green-400">Processing time: {processingTime}</span>
            {/if}
          </div>
        </div>
      {/if}
    </div>

    <!-- Additional info -->
    <div class="mt-4 rounded border border-zinc-700 bg-zinc-800 p-3">
      <h4 class="mb-2 text-sm font-medium text-zinc-300">About Hash Functions</h4>
      <ul class="space-y-1 text-xs text-zinc-400">
        <li>• SHA-256: Most common for file integrity</li>
        <li>• SHA-512: Occasionally used, but less common</li>
        <li>• MD5: Rare but still found sometimes</li>
      </ul>
    </div>
  </div>
</div>
