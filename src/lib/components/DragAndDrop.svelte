<script>
    import { fade, scale } from 'svelte/transition';
    
    let input;
    let container;
    let image;
    let placeholder;
    let showImage = false;
    let isDragging = false;
    export let file = null;

    function onChange() {
      file = input.files[0];
          
      if (file) {
        showImage = true;
  
        const reader = new FileReader();
        reader.addEventListener("load", function () {
          file = reader.result;
          image.setAttribute("src", reader.result);
        });
        reader.readAsDataURL(file);
        return;
      } 
      showImage = false; 
    }

    function handleDragOver(e) {
      e.preventDefault();
      isDragging = true;
    }

    function handleDragLeave() {
      isDragging = false;
    }

    function handleDrop(e) {
      e.preventDefault();
      isDragging = false;
      
      if (e.dataTransfer.files.length) {
        input.files = e.dataTransfer.files;
        onChange();
      }
    }

    function triggerFileInput() {
      input.click();
    }
</script>

<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div 
  class="w-full min-h-[200px] border-2 border-dashed rounded-lg mt-4 flex items-center justify-center font-bold text-white cursor-pointer transition-all duration-300 relative bg-white bg-opacity-10
         {isDragging ? 'border-white bg-opacity-20 scale-[1.02]' : 'border-gray-400 hover:border-white hover:bg-opacity-15'}
         {showImage ? 'border-solid bg-opacity-5' : ''}"
  on:dragover={handleDragOver}
  on:dragleave={handleDragLeave}
  on:drop={handleDrop}
  on:click={triggerFileInput}
  bind:this={container}
>
  <input
    bind:this={input}
    on:change={onChange}
    type="file"
    accept="image/*"
    class="hidden"
  />
  
  {#if showImage}
    <div class="w-full h-full relative" in:scale={{ duration: 300, start: 0.8 }}>
      <img bind:this={image} src="" alt="Preview" class="w-full max-h-[300px] object-contain rounded-md" />
      <button 
        class="absolute top-2 right-2 w-8 h-8 rounded-full bg-black bg-opacity-60 text-white flex items-center justify-center text-xl transition-all duration-200 hover:bg-red-500 hover:bg-opacity-80 hover:scale-110"
        on:click|stopPropagation={() => { showImage = false; file = null; }}
      >
        ×
      </button>
    </div>
  {:else}
    <div class="flex flex-col items-center justify-center p-5 text-center gap-2" in:fade={{ duration: 200 }}>
      <svg xmlns="http://www.w3.org/2000/svg" class="w-12 h-12 mb-2" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
        <polyline points="17 8 12 3 7 8"></polyline>
        <line x1="12" y1="3" x2="12" y2="15"></line>
      </svg>
      <span bind:this={placeholder} class="text-lg">
        คลิกหรือลากไฟล์มาที่นี่เพื่ออัปโหลด
      </span>
      <span class="text-sm opacity-70">รองรับไฟล์รูปภาพทุกประเภท</span>
    </div>
  {/if}
</div>