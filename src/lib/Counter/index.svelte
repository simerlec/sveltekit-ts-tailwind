<script lang="ts">
  import { spring } from 'svelte/motion';
  import { count as c } from './counterStore';

  let count = 0;

  const unsubscribe = c.subscribe((value) => {
    count = value;
  });

  const displayed_count = spring();
  $: displayed_count.set(count);
  $: offset = modulo($displayed_count, 1);

  function modulo(n: number, m: number) {
    // handle negative numbers
    return ((n % m) + m) % m;
  }
</script>

<div class="flex border-b border-t border-solid border-[#0000001a] my-1 mx-0">
  <button
    class="w-12 p-0 flex items-center justify-center bg-transparent border-none text-2xl hover:bg-[#d0dde9] text-[#444444]"
    on:click={() => c.update((n) => n - 1)}
    aria-label="Decrease the counter by one"
  >
    <svg aria-hidden="true" viewBox="0 0 1 1">
      <path d="M0,0.5 L1,0.5" />
    </svg>
  </button>

  <div class="counter-viewport">
    <div class="counter-digits" style="transform: translate(0, {100 * offset}%)">
      <strong style="top: -100%" aria-hidden="true">{Math.floor($displayed_count + 1)}</strong>
      <strong>{Math.floor($displayed_count)}</strong>
    </div>
  </div>

  <button
    class="w-12 p-0 flex items-center justify-center bg-transparent border-none text-2xl hover:bg-[#d0dde9] text-[#444444]"
    on:click={() => c.update((n) => n + 1)}
    aria-label="Increase the counter by one"
  >
    <svg aria-hidden="true" viewBox="0 0 1 1">
      <path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
    </svg>
  </button>
</div>

<style>
  svg {
    width: 25%;
    height: 25%;
  }

  path {
    vector-effect: non-scaling-stroke;
    stroke-width: 2px;
    stroke: var(--text-color);
  }

  .counter-viewport {
    width: 8em;
    height: 4em;
    overflow: hidden;
    text-align: center;
    position: relative;
  }

  .counter-viewport strong {
    position: absolute;
    display: block;
    width: 100%;
    height: 100%;
    font-weight: 400;
    color: var(--accent-color);
    font-size: 4rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .counter-digits {
    position: absolute;
    width: 100%;
    height: 100%;
  }
</style>
