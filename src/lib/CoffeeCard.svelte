<!-- coffee card component -->

<script lang="ts">
  import starIcon from "../assets/Star_fill.svg";
  import unFilledStarIcon from "../assets/Star.svg";
  import type { CoffeeItem } from "../types";

  export let coffeeItem: CoffeeItem;
  export const { available, id, image, name, popular, price, rating, votes } =
    coffeeItem;
</script>

<li class="coffee-card">
  <div class="image-container">
    <img src={image} alt={name} />
    {#if popular}
      <div class="popular">Popular</div>
    {/if}
  </div>
  <div class="coffee-info">
    <div class="name-price">
      <p class="name">
        {name}
      </p>
      <p class="price">
        {price}
      </p>
    </div>
    <div class="votes-sold-out">
      {#if votes > 0}
        <div class="votes">
          <img src={starIcon} alt="star icon" />
          <span class="rating">
            {rating}
          </span>
          <span class="votes-number">
            ({votes} votes)
          </span>
        </div>
      {:else}
        <div class="no-votes">
          <img src={unFilledStarIcon} alt="non filled star" />
          No votes yet
        </div>
      {/if}
      {#if !available}
        <span class="sold-out"> Sold Out </span>
      {/if}
    </div>
  </div>
</li>

<style lang="scss" scoped>
  @import "../vars.scss";

  .coffee-card {
    max-width: 16.5rem;
    .image-container {
      position: relative;
      width: 100%;
      .popular {
        position: absolute;
        top: 0.5rem;
        left: 0.5rem;
        background-color: $yellow;
        color: $charcoal;
        padding: 0.25rem 0.8rem;
        border-radius: 4rem;
        font-weight: 700;
        font-size: 0.8rem;
      }

      img {
        border-radius: 1rem;
      }
    }

    .coffee-info {
      .name-price {
        display: flex;
        align-items: center;
        justify-content: space-between;

        .price {
          background-color: $pale-green;
          color: $black;
          width: 3rem;
          padding-block: 0.2rem;
          text-align: center;
          font-size: 0.7rem;
          font-weight: 700;
          border-radius: 0.3rem;
        }
      }
    }

    .votes-sold-out {
      display: flex;
      justify-content: space-between;
      align-items: center;

      .votes {
        display: flex;
        align-items: center;

        .rating {
          margin-left: 0.2rem;
        }
        .votes-number {
          margin-left: 0.5rem;
          font-size: 0.9rem;
          color: $gray;
        }
      }

      .no-votes {
        display: flex;
        align-items: center;
        font-size: 0.8rem;
        color: $gray;
        gap: 0.4rem;
      }

      .sold-out {
        color: $red;
        font-size: 0.8rem;
        font-weight: 700;
      }
    }
  }
</style>
