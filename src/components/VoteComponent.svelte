<script>
  export let selectedPoll;
  export let username = 'defaultUser';

  // Vote on a specific option
  async function vote(optionId, voteType) {
    try {
      const selectedOption = selectedPoll.voteOptions.find(option => option.id === optionId);

      if (!selectedOption) {
        throw new Error('Selected option not found');
      }

      const voteData = {
        publishedAt: new Date().toISOString(),
        voteOption: selectedOption
      };

      const response = await fetch(`http://localhost:8080/users/${username}/votes`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(voteData),
      });

      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }

      const data = await response.json();
      console.log('Vote successful:', data);

    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>

<!-- Poll Display UI -->
<div class="poll-display">
  <h3 class="poll-question">{selectedPoll.question}</h3>

  <ul class="options">
    {#each selectedPoll.voteOptions as option}
      <li>
        <span class="option-text">{option.caption}</span>
        <button class="vote-button upvote" on:click={() => vote(option.id, 'upvote')}>upvote</button>
        <button class="vote-button downvote" on:click={() => vote(option.id, 'downvote')}>downvote</button>
        <span class="vote-count">{option.votes} Votes</span>
      </li>
    {/each}
  </ul>
</div>
