<script>
  import { onMount } from "svelte";
  import { ApolloClient, InMemoryCache } from "@apollo/client";
  import { setClient, query } from "svelte-apollo";
  import { gql } from "@apollo/client";

  let inputText = "";

  const client = new ApolloClient({
    uri: "https://guestbook-api.phocks.org/apollo",
    cache: new InMemoryCache(),
  });

  // Make Svelte work with ApolloClient
  setClient(client);

  const posts = query(gql`
    query {
      posts {
        text
      }
    }
  `);

  onMount(() => {
    // Do stuff on mount
    console.log(":)");
    return () => {
      // Do stuff on unmount
    };
  });

  function postText(event) {
    // console.log(event);
    console.log(inputText);
  }
</script>

<div class="App">
  <form on:submit|preventDefault={postText}>
    <input bind:value={inputText} placeholder="Enter a message" required />
    <button type="submit">Post</button>
  </form>

  {#if $posts.loading}
    <!-- Loading... -->
  {:else}
    <ul>
      {#each $posts.data.posts as post}
        <li>{post.text}</li>
      {/each}
    </ul>
  {/if}
</div>

<style>
  :global(body) {
    font-size: 16px;
    margin: 20px;
    font-family: "Work Sans", sans-serif;
    color: #333;
  }

  .App input {
    margin-bottom: 2em;
    font-size: 16px;
    font-size: max(16px, 1em);
    font-family: inherit;
    padding: 0.25em 0.5em;
    background-color: #fff;
    border: 1px solid rgb(139, 139, 139);
    border-radius: 0px;
  }

  .App button {
    font-size: 16px;
    font-size: max(16px, 1em);
    font-family: inherit;
    background-color: transparent;
    border: 1px solid rgb(139, 139, 139);
    border-radius: 0px;
    padding: 0.25em 0.5em;
  }

  .App ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .App ul li {
    margin-bottom: 1.6em;
  }
</style>
