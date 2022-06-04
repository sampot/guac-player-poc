<script lang="ts">
  import { onMount } from "svelte";
  import Guacamole from "guacamole-common-js";

  let displayElmt: any;
  let player: any;

  onMount(() => {
    console.log("the component has mounted");

    const tunnel = new Guacamole.StaticHTTPTunnel("/recording.guac");
    player = new Guacamole.SessionRecording(tunnel);

    // Add client to display div
    const display = player.getDisplay();
    displayElmt.appendChild(display.getElement());

    // Error handler
    player.onerror = function (error: any) {
      console.error(error);
    };

    player.onplay = () => {
      console.log("Playing");
    };

    player.onprogress = (duration: number) => {
      console.log("onProgress: ", duration);
    };

    // player.onseek = function (pos: number) {
    //   console.log("Seek: ", pos);
    // };

    player.onpause = () => {
      console.log("On pause");
    };

    // Connect
    player.connect();
    // player.play();
    console.log("Dureation: ", player.getDuration());
    console.log("Positiion: ", player.getPosition());
    console.log("isPlaying: ", player.isPlaying());
  });
</script>

<main>
  <h1>Guac Player</h1>
  <div>
    <button on:click={() => player.play()}>Play</button>
    <button on:click={() => player.pause()}>Pause</button>
    <button on:click={() => player.seek(0)}>Back(0)</button>
    <button on:click={() => player.seek(10000)}>Forward(10s)</button>
  </div>
  <div
    id="display"
    bind:this={displayElmt}
    style="min-width:640px;min-height:480px"
  />
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }

  img {
    height: 16rem;
    width: 16rem;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }
  }
</style>
