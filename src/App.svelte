<script>
  import { onMount } from "svelte";
  import Search from "./Search.svelte";
  import CardList from "./Cards.svelte";
  import Card from "./lib/components/CardProxy.svelte";

  let allCards = []; // Array to hold all cards
  let query = "";
  let isLoading = true;

  const getCards = async () => {
    let cardFetch = await fetch("/data/cards.json");
    let cards = await cardFetch.json();
    return cards;
  };

  const loadCards = async () => {
    return getCards()
      .then((cards) => {
        allCards = cards; // Store all cards in allCards array
        isLoading = false;
      });
  };

  onMount(() => {
    loadCards();
  });
</script>

<main>
  <header>
    <h1 id="⚓-top">Ollie & Benji</h1>
  </header>

  <Search bind:query />

  {#if query.length < 3}
    <CardList>
      {#if isLoading}
        loading...
      {:else}
        {#each allCards as card, index} <!-- Loop through all cards -->
          <Card
            id={card.id}
            name={card.name}
            number={card.number}
            set={card.set}
            types={card.types}
            supertype={card.supertype}
            subtypes={card.subtypes}
            rarity={card.rarity}
          />
        {/each}
      {/if}
    </CardList>
  {/if}
</main>

<div class="back-to-top">
  <a href="#⚓-top">Back to Top</a>
</div>

<style>
  .back-to-top a {
    color: inherit;
    text-decoration: none;
    z-index: 999;
  }
</style>
