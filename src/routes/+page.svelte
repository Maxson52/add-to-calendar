<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";
  import "add-to-calendar-button";
  import { goto } from "$app/navigation";

  let calendar_name = "";
  let calendar_url = "";
  onMount(() => {
    calendar_name =
      $page.url.searchParams.get("calendar_name") || "My New Calendar";
    calendar_url = $page.url.searchParams.get("calendar_url") || "";

    if (!calendar_url) {
      goto("/new?error=No calendar URL provided.");
    }
  });
</script>

<svelte:head>
  <title>Subscribe to {calendar_name}</title>
</svelte:head>

<div class="container">
  <h2>Subscribe to {calendar_name}</h2>

  <add-to-calendar-button
    name={calendar_name}
    subscribe
    icsFile={calendar_url}
    options="'Google','Apple','Outlook.com','Microsoft 365','Microsoft Teams','iCal'"
    buttonStyle="flat"
    hideIconList
    buttonsList
    hideBackground
    label="Flat and Singleton"
    lightMode="bodyScheme"
  ></add-to-calendar-button>

  <p style="margin-top: 1rem; margin-bottom: 0.25rem;">
    Don't see a button? Double check the URL or contact the calendar owner.
  </p>
  <a href="/new">Create a new calendar</a>
</div>

<style>
  .container {
    max-width: 600px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 4rem;
    border-radius: 1rem;
  }

  h2 {
    margin: 0 0 2rem 0;
  }
</style>
