<script lang="ts">
    import {fade} from "svelte/transition";
    import {tweened} from "svelte/motion";
    import {cubicOut} from "svelte/easing";

    const progress = tweened(0, {
      duration: 2500,
      easing: cubicOut,
    });

    progress.set(100)
</script>

<div class="background" out:fade|global={{duration: 500}}>
  <div class="progress" out:fade|global={{duration: 500}}>
    <div class="outer">
      <div class="inner" style="width: {$progress}%"></div>
    </div>
  </div>
  <img class="logo" src="img/menu/splash.svg" alt="logo" out:fade|global={{duration: 500}}>
</div>

<style lang="scss">
  @import "../../../colors.scss";

  .progress {
    z-index: 1;
    position: fixed;
    top: 60%;
    left: 50%;
    transform: translate(-50%, -50%);

    .inner, .outer {
      height: 3px;
      border-radius: 10px;
    }

    .outer {
      width: 250px;
      background-color: rgba(#202020, 1);
      border: solid 1px $border-thing;
      margin: 0px auto;
    }

    .inner {
      width: 50%;
      background-color: white;
    }
  }

  .background {
    position: absolute;
    left: 0px;
    top: 0px;
    background-color: black;
    width: 100vw;
    height: 100vw;
    z-index: -99;
  }

  .logo {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    }
</style>