<script>
    let input;
    let container;
    let image;
    let placeholder;
    let showImage = false;
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
  </script>
  <input
      bind:this={input}
      on:change={onChange}
    type="file"
  />
  <div bind:this={container}>
      {#if showImage}
          <img bind:this={image} src="" alt="Preview" />
      {:else}
          <span bind:this={placeholder}>Image Preview</span>
      {/if}
  </div>
  
  <style>
      div {
      width: 300px;
      min-height: 100px;
      border: 2px solid #ddd;
      margin-top: 15px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      color: white;
    }
    img {
      width: 100%;
    }
  </style>