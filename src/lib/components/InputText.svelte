<script lang="ts">
  export let values = "";
  export let onKeyUp = () => {};
  export let placeHolder = "";
</script>

<input
  class="one-word-input"
  type="text"
  bind:value={values}
  placeholder={placeHolder}
  on:keyup={() => onKeyUp()}
/>

<style>
  .one-word-input {
    width: 50%;
    padding: 8px;
  }
</style>
