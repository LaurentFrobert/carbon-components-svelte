<script>
  let className = undefined;
  export { className as class };
  export let heading = false;
  export let lines = 3;
  export let paragraph = false;
  export let style = undefined;
  export let width = '100%';

  import { cx } from '../../lib';

  const randoms = [0.973051493507435, 0.15334737213558558, 0.5671034553053769];

  let rows = [];

  $: widthNum = parseInt(width, 10);
  $: widthPx = width.includes('px');
  $: widthPercent = width.includes('%');
  $: if (paragraph) {
    for (let i = 0; i < lines; i++) {
      const min = widthPx ? widthNum - 75 : 0;
      const max = widthPx ? widthNum : 75;
      const randomWidth = Math.floor(randoms[i % 3] * (max - min + 1)) + min + 'px';
      rows = [...rows, { width: widthPx ? randomWidth : `calc(${width} - ${randomWidth})` }];
    }
  }
</script>

{#if paragraph}
  <div on:click on:mouseover on:mouseenter on:mouseleave class={className} {style}>
    {#each rows as { width }, i (width)}
      <p
        class={cx('--skeleton__text', heading && '--skeleton__heading')}
        style={`width: ${width};`} />
    {/each}
  </div>
{:else}
  <p
    on:click
    on:mouseover
    on:mouseenter
    on:mouseleave
    class={cx('--skeleton__text', heading && '--skeleton__heading', className)}
    style={`width: ${width};`}
     />
{/if}
