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
    background: #eee;
    padding: 1rem;
  }
</style>

<main>
  <Aform />
</main>
