<script lang="ts">
  import CreatePollComponent from './components/CreatePollComponent.svelte';
  import VoteComponent from './components/VoteComponent.svelte';
</script>

<main>
    <h1>Poll-Vote App</h1>
    <CreatePollComponent />
    <hr />
    <VoteComponent />
</main>

<style>
    main{
        max-width: 2000px;
        margin: 0 auto;
        padding: 20px;
    }
    hr{
        margin: 40px 0;
    }

    h1{
        font-size: 56px;
        text-align: center;
        color: #2f3a48;
        margin-bottom: 40px;
    }
</style>
