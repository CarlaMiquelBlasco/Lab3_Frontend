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

</script>
