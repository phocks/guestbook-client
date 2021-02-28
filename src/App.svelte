<script>
  import { onMount } from "svelte";
  import { ApolloClient, InMemoryCache } from "@apollo/client";
  import { setClient, query } from "svelte-apollo";
  import { gql } from "@apollo/client";

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
    return () => {
      // Do stuff on unmount
    };
  });
</script>

<div class="App">
  {#if $posts.loading}
    Loading...
  {:else}
    <ul>
      {#each $posts.data.posts as post}
        <li>{post.text}</li>
      {/each}
    </ul>
  {/if}
</div>

<style>
</style>
