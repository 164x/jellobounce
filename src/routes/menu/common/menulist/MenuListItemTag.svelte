<script lang="ts">
    export let text: string;
</script>

<div class="tag">
    {text}
</div>

<style lang="scss">
  @import "../../../../colors.scss";

  .tag {
    background-color: rgba($background-color, 0.36);
    color: $text-color;
    font-size: 12px;
    padding: 3px 10px;
    border-radius: 20px;
    margin-left: 10px;
    transition: ease color .2s, ease background-color .2s;
    transform: translateY(-3px);
    z-index: 1;
  }
</style>