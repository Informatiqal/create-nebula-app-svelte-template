<script lang="ts">
  import { QlikComponent } from "./lib/function/qlikSaas";
  import NebulaBar from "./lib/NebulaBar.svelte";
  import Spinner from "./lib/UI/spinner.svelte";
  import type { Nebula, QlikInstance } from "../../base/src/types";

  let fields = [`Valuelist("Dim1")`, `=sum(10)`];
  let qlikInstance: QlikComponent;

  const getQlikInstance = async (): Promise<Nebula> => {
    qlikInstance = new QlikComponent();
    return await qlikInstance.chart();
  };

  let promise: Promise<QlikInstance> = getQlikInstance().then((nebula) => {
    return { nebula, app: qlikInstance.app };
  });
</script>

<main>
  {#await promise}
    <Spinner />
  {:then data}
    <NebulaBar nebula={data.nebula} app={data.app} {fields} />
  {/await}
</main>

<style>
  @import "../global.css";
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }
</style>
