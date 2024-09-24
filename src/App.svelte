<script lang="ts">
  let isBilledAnnually = false;
  let prices = {
    yearly: [36, 72, 144, 360, 720],
    monthly: [4, 8, 16, 40, 80],
  };
  let pageViews = ["25K", "50K", "100K", "250K", "500K"];
  let currentRangeValue = 2;
  $: billingType = isBilledAnnually ? "yearly" : "monthly";
  $: currentPageViews = pageViews[currentRangeValue];
  $: currentPriceRange =
    prices[billingType as keyof typeof prices][currentRangeValue];
  $: currentPriceFormated = () =>
    new Intl.NumberFormat("en-US", {
      style: "currency",
      currency: "USD",
    }).format(currentPriceRange);
</script>

<main>
  <header class="header">
    <h1 class="title">Simple, traffic-based pricing</h1>
    <p class="description">
      Sign-up for our 30-day trial. No credit card required.
    </p>
  </header>
  <article class="pricing">
    <div class="calculator">
      <div class="pageviews">{currentPageViews} pageviews</div>
      <div class="price">
        <output class="price__value">{currentPriceFormated()}</output> / month
      </div>
      <input
        class="slider"
        data-value={currentRangeValue}
        type="range"
        min={0}
        bind:value={currentRangeValue}
        max={4}
      />
      <div class="billing">
        <div class="monthly">Monthly Billing</div>
        <label class="switch">
          <input
            class="checkbox"
            type="checkbox"
            bind:checked={isBilledAnnually}
          />
        </label>
        <div class="yearly">
          Yearly Billing <div class="discount">25% <span>discount</span></div>
        </div>
      </div>
    </div>
    <footer class="trial">
      <ul class="trial-benefits" role="list">
        {#each ["Unlimited websites", "100% data ownership", "Email reports"] as item}
          <li class="trial-benefits__item">{item}</li>
        {/each}
      </ul>
      <a class="trial-cta" href="#0">Start my trial</a>
    </footer>
  </article>
</main>

<style>
  main {
    min-block-size: 100dvh;
    align-content: center;
    isolation: isolate;
    padding-inline: 1.5rem;

    &::before {
      content: "";
      position: fixed;
      top: 0;
      left: 0;
      inline-size: 100vw;
      block-size: 50vh;
      border-end-start-radius: 20rem;
      z-index: -1;
      background-color: hsl(var(--grayish-blue));
      opacity: 0.1;
    }
  }

  .header {
    inline-size: min(33.75rem, 100%);
    margin-inline: auto;
    text-align: center;
    margin-block-end: 5rem;
    position: relative;
    isolation: isolate;

    &::before,
    &::after {
      content: "";
      position: absolute;
      z-index: -1;
      border-radius: 100vmax;
      border: 1px solid hsl(var(--grayish-blue));
      opacity: 0.25;
    }

    &::before {
      inline-size: 7.8125rem;
      block-size: 7.8125rem;
      top: 50%;
      left: 50%;
      translate: -50% -50%;
    }

    &::after {
      inline-size: 5rem;
      block-size: 5rem;
      top: 0;
      left: 50%;
      translate: 0% -65%;
    }
  }

  .title {
    color: hsl(var(--dark-desaturated-blue));
    font-weight: 700;
    font-size: 1.75rem;
    margin-block: 0;

    @media (width < 48rem) {
      font-size: 1.25rem;
    }
  }

  .description {
    font-size: 0.875rem;
    margin-block-start: 1em;
    text-wrap: balance;
  }

  .pricing {
    inline-size: min(33.75rem, 100%);
    margin-inline: auto;
    background: hsl(var(--white));
    box-shadow: hsl(var(--dark-desaturated-blue) / 0.1) 0px 0.5rem 2rem 0px;
    border-radius: 0.75em;
  }

  .calculator {
    padding: 2rem 3rem;
    display: grid;
    gap: 1.5rem;
    align-items: center;
    grid-template-columns: repeat(2, 1fr);
    grid-template-areas:
      "pageviews price"
      "slider slider"
      "billing billing";

    @media (width < 48rem) {
      padding: 2rem 1.5rem;
      justify-content: center;
      grid-template-columns: 1fr;
      grid-template-areas:
        "pageviews"
        "slider"
        "price"
        "billing";
    }
  }

  .pageviews {
    grid-area: pageviews;
    text-transform: uppercase;
    font-size: 0.875rem;
    font-weight: 600;
    letter-spacing: 0.17em;

    @media (width < 48rem) {
      font-size: 0.75rem;
      justify-self: center;
      text-align: center;
    }
  }

  .price {
    grid-area: price;
    text-align: right;
    line-height: 1;
    font-size: 1rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-self: end;
    gap: 0.5rem;

    @media (width < 48rem) {
      font-size: 0.75rem;
      justify-self: center;
      text-align: center;
    }
  }

  .price__value {
    font-size: 2rem;
    font-weight: 700;
    color: hsl(var(--dark-desaturated-blue));

    @media (width < 48rem) {
      font-size: 1.5rem;
    }
  }

  .slider {
    --slider-value: 0%;
    grid-area: slider;
    -webkit-appearance: none;
    appearance: none;
    background: transparent;
    cursor: pointer;
    margin-block: 1.25rem;

    &:is(:focus) {
      outline: transparent;
    }
  }

  .slider::-moz-range-track {
    border-radius: 100vmax;
    block-size: 0.5rem;
    background: linear-gradient(
      to right,
      hsl(var(--soft-cyan)) var(--slider-value),
      hsl(var(--light-grayish-blue)) var(--slider-value)
    );
  }

  .slider::-webkit-slider-runnable-track {
    border-radius: 100vmax;
    block-size: 0.5rem;
    background: linear-gradient(
      to right,
      hsl(var(--soft-cyan)) var(--slider-value),
      hsl(var(--light-grayish-blue)) var(--slider-value)
    );
  }

  .slider[data-value="1"]::-moz-range-track {
    --slider-value: 25%;
  }
  .slider[data-value="1"]::-webkit-slider-runnable-track {
    --slider-value: 25%;
  }
  .slider[data-value="2"]::-moz-range-track {
    --slider-value: 50%;
  }
  .slider[data-value="2"]::-webkit-slider-runnable-track {
    --slider-value: 50%;
  }
  .slider[data-value="3"]::-moz-range-track {
    --slider-value: 75%;
  }
  .slider[data-value="3"]::-webkit-slider-runnable-track {
    --slider-value: 75%;
  }
  .slider[data-value="4"]::-moz-range-track {
    --slider-value: 100%;
  }
  .slider[data-value="4"]::-webkit-slider-runnable-track {
    --slider-value: 100%;
  }

  .slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    border-radius: 100vmax;
    block-size: 2.5rem;
    inline-size: 2.5rem;
    background:
      url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='22' height='13'%3E%3Cg fill='%2380FFF3' fill-rule='evenodd'%3E%3Cpath d='M16 2.558v7.884a1 1 0 001.735.679l3.639-3.943a1 1 0 000-1.356l-3.64-3.943A1 1 0 0016 2.558zM6 2.558v7.884a1 1 0 01-1.735.679L.626 7.178a1 1 0 010-1.356l3.64-3.943A1 1 0 016 2.558z'/%3E%3C/g%3E%3C/svg%3E")
        no-repeat center,
      hsl(var(--strong-cyan));
    box-shadow: 0px 1rem 1rem 0rem hsl(var(--strong-cyan) / 0.5);
    margin-block-start: -1rem;

    @media (width < 48rem) {
      box-shadow: 0px 0.75rem 1rem 0rem hsl(var(--strong-cyan) / 0.5);
      block-size: 2rem;
      inline-size: 2rem;
      margin-block-start: -0.75rem;
      background-size: 1.125rem;
    }
  }

  .slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    border-radius: 100vmax;
    block-size: 2.5rem;
    inline-size: 2.5rem;
    margin-block-start: -1rem;
    background:
      url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='22' height='13'%3E%3Cg fill='%2380FFF3' fill-rule='evenodd'%3E%3Cpath d='M16 2.558v7.884a1 1 0 001.735.679l3.639-3.943a1 1 0 000-1.356l-3.64-3.943A1 1 0 0016 2.558zM6 2.558v7.884a1 1 0 01-1.735.679L.626 7.178a1 1 0 010-1.356l3.64-3.943A1 1 0 016 2.558z'/%3E%3C/g%3E%3C/svg%3E")
        no-repeat center,
      hsl(var(--strong-cyan));
    box-shadow: 0px 1rem 1rem 0rem hsl(var(--strong-cyan) / 0.5);

    @media (width < 48rem) {
      box-shadow: 0px 0.75rem 1rem 0rem hsl(var(--strong-cyan) / 0.5);
      block-size: 2rem;
      inline-size: 2rem;
      margin-block-start: -0.75rem;
      background-size: 1.125rem;
    }
  }

  .billing {
    grid-area: billing;
    display: flex;
    align-items: center;
    gap: 0.75em;
    justify-content: center;
  }

  .checkbox {
    display: none;
  }

  .switch {
    inline-size: 3rem;
    block-size: auto;
    aspect-ratio: 16 / 9;
    position: relative;
    cursor: pointer;

    @media (width < 48rem) {
      inline-size: 2.5rem;
    }

    &::before,
    &::after {
      content: "";
      position: absolute;
      display: block;
      border-radius: 100vmax;
    }

    &::before {
      inset: 0;
      background-color: hsl(var(--light-grayish-blue));
    }

    &::after {
      aspect-ratio: 1;
      inset: 0.25rem auto 0.25rem 0.25rem;
      background-color: hsl(var(--white));
      transition: all 0.2s ease-in-out;
    }

    &:has(input:checked) {
      &::before {
        background-color: hsl(var(--strong-cyan));
      }
      &::after {
        inset: 0.25rem 0.25rem 0.25rem auto;
      }
    }
  }

  .yearly {
    position: relative;
  }

  .discount {
    position: absolute;
    top: 50%;
    right: 0;
    translate: calc(100% + 1em) -50%;
    padding: 0.25em 0.75em;
    border-radius: 100vmax;
    background-color: hsl(var(--light-grayish-red));
    color: hsl(var(--light-red));
    font-size: 0.625rem;
    white-space: nowrap;

    @media (width < 48rem) {
      translate: calc(100% + 0.5em) -50%;

      & span {
        display: none;
      }
    }
  }

  .trial {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 2rem;
    padding: 2rem 3rem;
    box-shadow: 0 -1px 0 0 hsl(var(--dark-desaturated-blue) / 0.1);

    @media (width < 48rem) {
      flex-direction: column;
      align-items: center;
    }
  }

  .trial-benefits {
    padding: 0;
    margin-block: 0;
    list-style-type: none;
  }

  .trial-benefits__item {
    display: flex;
    align-items: center;
    gap: 1em;

    &::before {
      --size: 0.75em;
      content: "";
      display: inline;
      inline-size: var(--size);
      block-size: var(--size);
      background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='9' height='8'%3E%3Cpath fill='none' stroke='%2310D8C4' stroke-width='2' d='M1 4.134l1.907 1.908L7.949 1'/%3E%3C/svg%3E")
        no-repeat center / var(--size);
    }

    &:not(:last-of-type) {
      margin-block-end: 0.5em;
    }
  }

  .trial-cta {
    padding: 1em 3em;
    border-radius: 100vmax;
    background-color: hsl(var(--dark-desaturated-blue));
    color: hsl(var(--pale-blue));
    text-decoration: none;
    transition: color 0.15s linear;
    font-weight: 600;

    &:is(:hover, :focus-visible) {
      color: hsl(var(--white));
    }
  }
</style>
