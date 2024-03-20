<script lang="ts">
</script>

<main>
  <h1>
    Hello world!
    <span class="active">Active</span>
  </h1>
</main>

<style lang="scss" scoped>

  @import 'vars.scss';

  $custom-blue: #0000ff;

  h1 {
    color: $charcoal;
    text-align: center;
    span {
      &.active {
        color: $red;
      }
    }
  }
</style>
