<script>
  import { createEventDispatcher } from 'svelte';

  let question = '';
  let optionInput = '';
  let options = [];
  const dispatch = createEventDispatcher();

  // Add option to poll
  function addOption() {
    if (optionInput.trim()) {
      options = [...options, { caption: optionInput, votes: 0 }];
      optionInput = '';
    }
  }

  // Create poll and send it to the parent component
  async function createPoll() {
    if (question.trim() && options.length) {
      try {
        const response = await fetch('http://localhost:8080/polls', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            question,
            publishedAt: new Date().toISOString(),
            validUntil: new Date(new Date().getTime() + 24 * 60 * 60 * 1000).toISOString(),
            voteOptions: options,
          }),
        });

        if (response.ok) {
          const createdPoll = await response.json();
          dispatch('pollCreated', createdPoll); // Dispatch the new poll to the parent
          question = '';
          options = [];
          alert('Poll successfully created!');  
        } else {
          alert('Failed to create poll');  
        }
      } catch (error) {
        console.error('Error creating poll:', error);
        alert('Error creating poll');  
      }
    } else {
      alert('Please enter a question and at least one option'); 
    }
  }
</script>

<!-- Poll Creation UI -->
<div class="poll-creation">
  <h2>Create a Poll</h2>
  <input type="text" bind:value={question} placeholder="Enter your question" />
  
  <h3>Options</h3>
  <input type="text" bind:value={optionInput} placeholder="Enter an option" />
  <button on:click={addOption}>Add Option</button>

  <ul>
    {#each options as option}
      <li>{option.caption}</li>
    {/each}
  </ul>

  <button on:click={createPoll}>Create Poll</button>
</div>

<style>
  /* Your styles here */
</style>
