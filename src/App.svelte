<script lang="ts">
  import { onMount } from "svelte";
  import dayjs from "dayjs";
  const keyListSentence = "list-sentect-key";
  let word = "";
  let sentence = "";
  let keyword = "";
  $: errorDuplicate = "";
  $: listSentence = [
    {
      word: "keng",
      sentence: "keng is the new people in the world he is a developer at ODDS",
      date: new Date(2022, 7, 10, 11, 11, 10),
    },
  ];
  $: wordCount = 0; // state

  const onSentenceChange = () => {
    wordCount = sentence.split(" ").filter((e) => e.length > 0).length;
  };

  onMount(async () => {
    const list = window.localStorage.getItem(keyListSentence);
    if (list) {
      listSentence = JSON.parse(list);
    }
  });

  const onInputKeyDown = () => {
    errorDuplicate = "";
    const found = listSentence.find(
      (e) => e.word.trim().toLowerCase() === word.trim().toLowerCase()
    );
    if (found) {
      errorDuplicate = "this word is existing";
    }
  };

  const onSave = () => {
    if (word.length && sentence.length) {
      listSentence = [
        {
          word: word.trim(),
          sentence,
          date: new Date(),
        },
        ...listSentence,
      ];
      word = "";
      sentence = "";
      localStorage.setItem(keyListSentence, JSON.stringify(listSentence));
    }
  };

  const onSearch = () => {
    const list = window.localStorage.getItem(keyListSentence);
    console.log(keyword, list);
    const filter = JSON.parse(list).filter((e) =>
      e.word.toLowerCase().includes(keyword)
    );
    listSentence = [...filter];
  };
</script>

<main>
  <div class="container">
    <p>What is the word for today</p>
    <input
      type="text"
      bind:value={word}
      class="one-word-input"
      placeholder="What is the word for today"
      on:keyup={() => onInputKeyDown()}
    />
    <span class="error">{errorDuplicate}</span>
    <div class="word-to-senten">
      <textarea
        id="sentence"
        cols="50"
        rows="5"
        bind:value={sentence}
        on:keydown={() => onSentenceChange()}
      />
      <span class="number-of-word">Number of word: {wordCount}</span>
      <div class="save-container">
        <button
          class="px"
          on:click={() => onSave()}
          disabled={wordCount < 20 ||
            word.length === 0 ||
            errorDuplicate.length > 0}>Save</button
        >
      </div>
    </div>
    <div class="sentence-list">
      <div class="seach-list-container">
        <input
          bind:value={keyword}
          type="text"
          on:keyup={() => onSearch()}
          class="search-input"
          placeholder="Seacrh word"
        />
      </div>
      {#if listSentence.length > 0}
        {#each listSentence as item}
          <hr style="border: 1px dashed black;" />
          <div class="list">
            <div class="title">
              {item.word.charAt(0).toUpperCase() + item.word.slice(1)}
            </div>
            <span class="when">{dayjs(item.date).locale("th").toString()}</span>
            <span>{item.sentence}</span>
          </div>
        {/each}
      {:else}
        <div class="not-found-title">Content not found</div>
      {/if}
    </div>
  </div>
</main>

<style>
  .not-found-title {
    margin-top: 8px;
  }
  .search-input {
    width: 30%;
    padding: 8px;
  }
  .error {
    font-size: 14px;
    color: rgb(232, 84, 84);
  }
  .sentence-list {
    width: 100%;
    margin-top: 32px;
    overflow: auto;
    height: 350px;
  }

  .list {
    display: flex;
    flex-direction: column;
    padding-top: 8px;
    margin-bottom: 14px;
  }

  .container {
    width: 100%;
    flex-direction: column;
    display: flex;
  }
  .when {
    font-size: 12px;
  }
  .title {
    font-size: 20px;
    font-weight: 600;
  }
  .px {
    padding-left: 32px;
    padding-right: 32px;
  }
  .save-container {
    width: 100%;
    display: flex;
    flex-direction: row-reverse;
  }
  .number-of-word {
    margin-top: 8px;
  }
  .word-to-senten {
    flex-direction: column;
    display: flex;
    margin-top: 8px;
  }
  .one-word-input {
    width: 50%;
    padding: 8px;
  }
  #sentence {
    font-size: 20px;
    width: 90%;
    padding: 16px;
  }
</style>
