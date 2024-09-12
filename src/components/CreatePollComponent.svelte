<script>
  let pollQuestion = "";
  let pollOptions = [{ caption: "" }];

  // Function to add a new option
  function addOption() {
    pollOptions = [...pollOptions, { caption: "" }];
  }

  // Function to remove an option
  function removeOption(index) {
    pollOptions = pollOptions.filter((_, i) => i !== index);
  }

  async function submitPoll() {
    const pollData = {
      question: pollQuestion,
      voteOptions: pollOptions.map((option, index) => ({
        caption: option.caption,
        presentationOrder: index + 1,
        upvote: 0
      }))
    };

    try {
      const response = await fetch('http://localhost:8080/polls', {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(pollData)
      });

      if (response.ok) {
        const data = await response.json();
        alert("Poll created successfully with ID: " + data.pollId + ". Now refresh the page to vote.");
      } else {
        console.error("Error creating poll:", response.statusText);
        alert("Failed to create poll");
      }
    } catch (error) {
      console.error("Error creating poll:", error);
      alert("Error creating poll");
    }
  }
</script>

<h1 class="app-title">Create a New Poll</h1>

<div class="poll-container">
  <form class="poll-form" on:submit|preventDefault={submitPoll}>
    <div class="form-group">
      <label for="pollQuestion">Poll Question:</label>
      <input type="text" id="pollQuestion" bind:value={pollQuestion} placeholder="Enter poll question.." />
    </div>

    <div class="options-section">
      <h3>Options</h3>
      {#each pollOptions as option, index}
        <div class="option-item">
          <input type="text" bind:value={option.caption} placeholder="Option {index + 1}" class="option-input" />
          {#if pollOptions.length > 1}
            <button type="button" class="remove-btn" on:click={() => removeOption(index)}>Remove</button>
          {/if}
        </div>
      {/each}
    </div>

    <div class="actions">
      <button type="button" class="add-btn" on:click={addOption}>Add Option</button>
      <button type="submit" class="create-btn">Create Poll</button>
    </div>
  </form>
</div>


<style>
  /* Container Styles */
  .poll-container {
    max-width: 1000px;
    margin: 20px auto;
    padding: 20px;
    border-radius: 10px;
    background-color: #fdeff5;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  /* Title and Section Headers */
  h1 {
    font-size: 36px;
    text-align: center;
    color: #2f3a48;
    margin-bottom: 40px;
  }

  h3 {
    font-size: 22px;
    color: #4f5a6a;
    margin-bottom: 15px;
    text-align: left;
  }

  /* Form Styles */
  .poll-form {
    display: flex;
    flex-direction: column;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-bottom: 20px;
  }

  label {
    font-size: 22px;
    font-weight: 600;
    margin-bottom: 8px;
    color: #4f5a6a;
  }

  input[type="text"] {
    width: 100%;
    padding: 10px;
    font-size: 16px;
    border-radius: 6px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }

  .options-section {
    margin-bottom: 20px;
    text-align: left;
  }

  .option-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  .option-input {
    flex-grow: 1;
    margin-right: 10px;
    padding: 10px;
    border-radius: 6px;
    border: 1px solid #ccc;
  }

  /* Button Styles */
  button {
    font-size: 16px;
    padding: 10px 15px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .add-btn {
    background-color: #f69eca;
    color: white;
    margin-right: 10px;
    font-weight: bold;

  }

  .create-btn {
    background-color: #f69eca;
    color: white;
    margin-right: 10px;
    font-weight: bold;
  }

</style>
