<script lang="ts">
  import { Meteor } from "meteor/meteor";
  import { LinksCollection, type Link } from '../api/links';
  
  let counter: number = 0;
  const addToCounter = (): void => {
    counter += 1;
  }
  
  let subIsReady: boolean = false;
  $m: {
    const handle: Meteor.SubscriptionHandle = Meteor.subscribe("links.all");
    subIsReady = handle.ready();
  }

  // more information about $m at https://atmospherejs.com/zodern/melte#tracker-statements
  let links: Link[];
  $m: links = LinksCollection.find().fetch();
</script>


<div class="container">
  <h1>Welcome to Meteor!</h1>

  <button on:click={addToCounter}>Click Me</button>
  <p>You've pressed the button {counter} times.</p>

  <h2>Learn Meteor!</h2>
  {#if subIsReady}
    <ul>
      {#each links as link (link._id)}
        <li><a href={link.url} target="_blank" rel="noreferrer">{link.title}</a></li>
      {/each}
    </ul>
  {:else}
    <div>Loading ...</div>  
  {/if}
</div>
