<script>
  import CreatePollComponent from './components/CreatePollComponent.svelte';
  import VoteComponent from './components/VoteComponent.svelte';

  let currentPoll = null; // Holder den nåværende opprettede pollen
  let pollCreated = false; // Kontrollvariabel for om en poll er opprettet

  // Håndterer opprettet poll-event fra CreatePollComponent
  function handlePollCreated(event) {
    currentPoll = event.detail; // Poll-data kommer fra event.detail
    pollCreated = true; // Sett pollCreated til true for å vise VoteComponent
  }
</script>

<main>
  <h1>Poll App</h1>
  
  <!-- Poll opprettingskomponent -->
  <CreatePollComponent on:pollCreated={handlePollCreated} />
  
  <!-- Hvis en poll er opprettet, vis VoteComponent -->
  {#if pollCreated && currentPoll}
  <VoteComponent selectedPoll={currentPoll} />
  {/if}
</main>
