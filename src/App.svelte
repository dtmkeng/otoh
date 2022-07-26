<script lang="ts">
  let word ="";
  let sentence ="";
  $: listSentence = [{word: "keng", sentence: "keng is the new people in the world he is a developer at ODDS", date: new Date(2022, 7, 10, 11, 11, 10)}];
  $: wordCount = 0; // state

  const onSentenceChange = () => {
    wordCount = sentence.split(' ').filter(e => e.length > 0).length;
  }

  const onSave = () => {
    listSentence = [{
      word,
      sentence,
      date: new Date()
    }, ...listSentence];
    word = "";
    sentence = "";
    console.log(listSentence)
  }
</script>

<main>
  <div>
    <p>What is the word for today</p>
    <input type="text" bind:value={word} class="one-word-input" placeholder="What is the word for today">
    <div class="word-to-senten">
      <textarea  id="sentence" cols="100" rows="10" bind:value={sentence} on:keydown={() => onSentenceChange()}></textarea>
      <span class="number-of-word">Number of word: {wordCount}</span>
      <div class="save-container">
        <button class="px" on:click={() => onSave()}>Save</button>
      </div>
    </div>
    <div>
      {#each listSentence as item}
        <div>
          <div class="title">{item.word}</div>
          <span class="when">{item.date}</span>
          <p>{item.sentence}</p>
        </div>
      {/each}
    </div>
  </div>
</main>

<style>
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
    padding: 16px;
  }
</style>