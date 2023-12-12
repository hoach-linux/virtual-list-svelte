<script lang="ts">
  import { onDestroy, onMount } from "svelte";

  let list: number[] = [];
  let showList: number[] = [];
  let itemHeight: number = 50;
  let itemMargin: number = 10;
  let listHeight: number = 0;
  let marginTop: number = 0;
  let h1Height: number = 39;
  let h1Margin: number = 40;
  let renderList = () =>
    renderItems(h1Height, h1Margin, itemHeight, itemMargin);

  function addListItems(quantity: number) {
    for (let i = 0; i <= quantity; i++) {
      list.push(i);
    }
  }
  function renderItems(
    h1Height: number,
    h1Margin: number,
    itemHeight: number,
    itemMargin: number
  ) {
    let maxItems = Math.round(
      (window.innerHeight - (h1Height + h1Margin)) / (itemHeight + itemMargin) +
        1
    );
    let top = window.scrollY;
    let startItem = Math.floor(top / (itemHeight + itemMargin));
    let endItem = startItem + maxItems;

    marginTop = window.scrollY;

    showList = list.slice(startItem, endItem);
  }

  onMount(() => {
    addListItems(9999);
    renderList();

    listHeight = (itemHeight + itemMargin) * list.length;

    if (typeof window !== "undefined") {
      window.addEventListener("scroll", renderList);
    }
  });
  onDestroy(() => {
    if (typeof window !== "undefined") {
      window.removeEventListener("scroll", renderList);
    }
  });
</script>

<main>
  <h1 style="text-align: center; margin-bottom: 100px">Virtual list</h1>
  <div
    class="virtual-list"
    style="height: {listHeight -
      marginTop +
      itemHeight +
      itemHeight}px; margin-top: {marginTop - (itemHeight + itemMargin)}px"
  >
    {#each showList as listItem, i}
      <div
        style="background: black; margin: 5px; color: #fff; text-align: center; padding: 5px; min-height: {itemHeight}px"
      >
        {listItem}
      </div>
    {/each}
  </div>
</main>
