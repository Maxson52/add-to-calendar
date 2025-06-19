<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";
  import "add-to-calendar-button";

  let error = null;
  onMount(() => {
    error = $page.url.searchParams.get("error");
  });

  let name = "";
  let url = "";
  let generatedLink = "";
  let copied = false;

  function generateLink() {
    if (!name || !url) return;

    const encodedName = encodeURIComponent(name.trim());
    const encodedUrl = encodeURIComponent(url.trim());
    generatedLink = `${window.location.origin}/?calendar_name=${encodedName}&calendar_url=${encodedUrl}`;
  }
</script>

<svelte:head>
  <title>Create a Calendar Invite Link</title>
</svelte:head>

<div class="container">
  <h2>Create a Calendar Invite Link</h2>

  <p class="error">{error}</p>

  <label for="name">Calendar Name</label>
  <input
    id="name"
    type="text"
    bind:value={name}
    placeholder="e.g. My Calendar"
  />

  <label for="url">.ics URL</label>
  <input
    id="url"
    type="text"
    bind:value={url}
    placeholder="Paste your .ics URL here"
  />

  <button on:click={generateLink} disabled={!name || !url}>
    Generate Link
  </button>

  {#if generatedLink}
    <p style="margin-top: 1rem; margin-bottom: 0rem;">Your link:</p>
    <code>{generatedLink}</code>

    <button
      style="margin-top: 0.5rem; margin-bottom: 0.25rem;"
      on:click={() =>
        navigator.clipboard.writeText(generatedLink).then(() => {
          copied = true;
          setTimeout(() => (copied = false), 5000);
        })}
    >
      Copy Link
    </button>

    {#if copied}
      <p class="copied">✅ Copied to clipboard!</p>
    {/if}
  {/if}
</div>

<style>
  .container {
    max-width: 600px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 2rem 4rem 4rem 4rem;
    border-radius: 1rem;
  }

  .copied {
    color: #4caf50;
  }

  .error {
    color: #ff0000;
  }

  code {
    display: block;
    background-color: #f4f4f4;
    padding: 10px;
    border-radius: 5px;
    margin-top: 10px;
    word-wrap: break-word;
  }
</style>
