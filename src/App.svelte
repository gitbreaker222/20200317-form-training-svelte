<script>
  import Aform from './Aform.svelte'

  let name = 'world'
</script>

<style>
  :global(*) {
    box-sizing: border-box;
  }

  main {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: auto;
    color: #333;
    background: #fff;
    padding: 1rem;
  }
</style>

<svelte:head>
  <link rel="stylesheet" href="formstyles.css" />
</svelte:head>

<main>
  <Aform />
</main>
