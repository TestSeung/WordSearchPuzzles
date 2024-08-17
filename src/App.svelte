<script>
  import Main from "./pages/Main.svelte";
  import Router from "svelte-spa-router";

  const routes = {
    "/": Main, //예외처리
  };
</script>

<Router {routes} />
