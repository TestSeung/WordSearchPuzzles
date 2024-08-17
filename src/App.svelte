<script>
  import Main from "./pages/Main.svelte";
  import Router from "svelte-spa-router";

  const routes = {
    "/": Main, //예외처리
  };
</script>

<link rel="stylesheet" href="pages/reset.css" />
<!-- path에 따른 라우팅 -->
<Router {routes} />
