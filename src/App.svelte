<script>
  import Main from "./pages/Main.svelte";
  import Lobby from "./pages/Lobby.svelte";
  import Router from "svelte-spa-router";

  const routes = {
    "/": Lobby,
    "/main": Main, //예외처리
  };

  async function hello() {
    const res = await fetch("<http://127.0.0.1:8000/hello>");
    const json = await res.json();

    if (res.ok) {
      return json.message;
    } else {
      alert("error");
    }
  }

  let promise = hello();
</script>

<!-- path에 따른 라우팅 -->
<Router {routes} />

{#await promise}
  <p>...waiting</p>
{:then message}
  <h1>{message}</h1>
{/await}
