<svelet:head>
  <meta http-equiv="refresh" content="30">
</svelet:head>
<script context="module">
  export function load({ error, status }) {
    return {
      props: {
        title: status,
      }
    };
  }
</script>

<script>
  export let title;
</script>

<main>
  <div class="number">{title}</div>
  <div class="text">Not your fault,its a server error🤖</div>
  <section>
    <a href="/">back to home page</a>
    <a href="javascript:location.reload(true)">Refresh this page</a>
  </section>
</main>


<style>
main {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  user-select: none;
}

.number {
  text-align: center;
  margin-top: 1rem;
  width: 100%;
  height: 100%;
  font: 900 20rem "Consolas";
  letter-spacing: 5vmin;
  text-shadow: 2px -1px 0 #0d1216, 4px -2px 0 #ffdf00, 6px -3px 0 #ffdf00, 8px -4px 0 #ffdf00, 10px -5px 0 #ffdf00, 12px -6px 0 #1f1f1f, 14px -7px 0 #242424, 16px -8px 0 #ffdf00;
}
.text{
  text-transform: capitalize;
  margin-top: -3rem;
  font-size: 2rem;
  text-shadow: 2px -1px 0 #0d1216, 2px -2px 0 #ffdf00;
  text-align: center;
}
section{
  margin-top: 2rem;
}
section a{
    text-align: center;
    border: 1px solid #ffdf00;
    outline: none;
    background-color: transparent;
    padding:0.4rem 0.8rem;
    border-radius: 2px;
    font-family: inherit;
    text-transform: capitalize;
    margin: 0.5rem auto;
    cursor: pointer;
    user-select: none;
    margin-right: 2rem;
    box-shadow: 2px -1px 0 #0d1216, 4px -2px 0 #ffdf00, 6px -3px 0 #ffdf00, 8px -4px 0 #ffdf00;
    color: #ffdf00;
    transition: 0.2s box-shadow ease-out;
}
section a:hover{
    box-shadow: 2px -1px 0 #0d1216, 4px -2px 0 #ffdf00;
    color: #0d1216;
    background-color: #ffdf00;
  }
</style>