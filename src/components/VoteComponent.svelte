<script>
  let polls = [];  // Holds all the polls
  let selectedPollId = null;  // Selected poll ID from dropdown
  let selectedPoll = null;  // Holds the details of the selected poll

  // Function to fetch all polls
  async function fetchAllPolls() {
    try {
      const response = await fetch("http://localhost:8080/polls");
      if (response.ok) {
        polls = await response.json();  // Fetch all polls
      } else {
        console.error("Failed to fetch polls:", response.statusText);
      }
    } catch (error) {
      console.error("Error fetching polls:", error);
    }
  }

  // Function to fetch a selected poll by pollId
  async function fetchSelectedPoll(pollId) {
    try {
      const response = await fetch(`http://localhost:8080/polls/${pollId}`);
      if (response.ok) {
        selectedPoll = await response.json();  // Fetch poll by ID
      } else {
        console.error("Failed to fetch poll:", response.statusText);
      }
    } catch (error) {
      console.error("Error fetching poll:", error);
    }
  }

  // Fetch all polls when the component is mounted
  fetchAllPolls();

  // Function to cast a vote for a specific option
  async function upvote(option) {
    const voteData = {
      voteOption: {
        caption: option.caption,
        presentationOrder: option.presentationOrder,
        upvote: option.upvote
      },
      publishedAt: new Date().toISOString()
    };

    try {
      const response = await fetch(`http://localhost:8080/votes/${selectedPoll.pollId}`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(voteData)
      });

      if (response.ok) {
        // Increment the votes locally after a successful vote
        option.upvote = (option.upvote || 0) + 1;
        alert(`Vote cast successfully! There are now ${option.upvote} votes for ${option.caption}.`);
      } else {
        console.error("Failed to cast vote:", response.statusText);
      }
    } catch (error) {
      console.error("Error casting vote:", error);
    }
  }

</script>
<h2>Select a Poll</h2>

<div>
  <label for="pollSelect">Choose a poll:</label>
  <select id="pollSelect" bind:value={selectedPollId} on:change={() => fetchSelectedPoll(selectedPollId)}>
    <option value="" disabled>Select Poll</option>
    {#each polls as poll}
      <option value={poll.pollId}>{poll.question}</option>
    {/each}
  </select>
</div>

{#if selectedPoll}
  <h3>Poll #{selectedPoll.pollId}</h3>
  <blockquote>{selectedPoll.question}</blockquote>
  <ul>
    {#each selectedPoll.voteOptions as option}
      <li>
        <div class="option">{option.caption}</div>
        <div class="buttons">
          <button class="upvote" on:click={() => upvote(option)}>upvote</button>
        </div>
        <span>{option.upvote} Votes</span>
      </li>
    {/each}
  </ul>
{/if}


<style>
  h2 {
    font-size: 36px;
    text-align: center;
    color: #2f3a48;
    margin-bottom: 40px;
  }

  h3 {
    font-size: 10px;
    text-align: left;
    color: #2f3a48;
    margin-bottom: 40px;
  }

  .poll-container {
    border: 5px solid #fdeff5;
    border-radius: 5px;
    padding: 15px;
    margin: 5px 0;
    max-width: 1000px;
    width: 100%;
    margin: auto;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  blockquote {
    background-color: #fdeff5;
    padding: 15px;
    margin: 0 0 20px 0;
    font-size: 28px;
    color: #2f3a48;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  li {
    display: grid;
    grid-template-columns: 2fr 3fr 1fr;
    gap: 15px;
    align-items: center;
    margin-bottom: 20px;
    font-size: 25px;
    padding: 15px;
    background-color: #fafafa;
    border-radius: 8px;
  }

  .option {
    font-weight: 600;
    text-align: left;
  }

  .buttons {
    display: flex;
    justify-content: flex-start;
    gap: 5px; /* Decrease this value to bring the buttons closer */
  }

  .upvote, .downvote {
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 20px;
  }

  .upvote {
    background-color: #8bc34a;
    color: white;
  }

  .downvote {
    background-color: #f44336;
    color: white;
  }

  .votes {
    font-weight: bold;
    text-align: right;
  }
</style>
