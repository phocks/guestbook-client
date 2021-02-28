<script lang='typescript'>
  import { onMount } from "svelte";
  import { ApolloClient, InMemoryCache } from "@apollo/client";
  import { setClient, query } from "svelte-apollo";
  import { gql } from "@apollo/client";

  const client = new ApolloClient({
    uri: "https://guestbook-api.phocks.org/apollo/",
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

  console.log(posts);




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
    {#each $posts.data.posts as post}
      {post.text}
    {/each}
  {/if}
</div>

<style>
</style>
