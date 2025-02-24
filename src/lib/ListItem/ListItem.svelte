<script lang="ts">
  import Accordion from '$lib/Accordion/Accordion.svelte';
  import Loader from '$lib/Loader/Loader.svelte';
  import Img from '$lib/Img/Img.svelte';
  import { defaultListItemProperties, type ListItemProperties } from './properties';
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let properties: ListItemProperties = defaultListItemProperties;
  export let showLoader = false;
  export let showRightContentLoader = false;
  export let expand = false;
  export let preventFocus = false;

  function handleLeftImageClick(): void {
    dispatch('leftImageClick');
  }

  function handleRightImageClick(): void {
    dispatch('rightImageClick');
  }

  function handleCenterTextClick(): void {
    dispatch('centerTextClick');
  }

  function handleItemClick(): void {
    dispatch('itemClick');
  }

  function handleTopSectionClick(): void {
    dispatch('topSectionClick');
  }
</script>

{#if properties.leftImageUrl || properties.rightImageUrl || properties.label || $$slots.leftContent || $$slots.centerContent || $$slots.rightContent || $$slots.bottomContent}
  <div class="item-container">
    {#if showLoader}
      <div class="item-loader" />
    {/if}
    <div
      class="item"
      class:prevent-focus={preventFocus}
      on:click={handleItemClick}
      on:keydown
      role="button"
      tabindex="0"
    >
      <div
        class="top-section"
        class:prevent-focus={preventFocus}
        on:click={handleTopSectionClick}
        on:keydown
        role="button"
        tabindex="0"
      >
        <div class="left-content">
          {#if properties.leftImageUrl}
            <div
              class:prevent-focus={preventFocus}
              on:click={handleLeftImageClick}
              on:keydown
              role="button"
              tabindex="0"
            >
              <Img
                src={properties.leftImageUrl}
                alt=""
                fallback={properties.leftImageFallbackUrl}
              />
            </div>
          {/if}
          {#if $$slots.leftContent}
            <slot name="leftContent" />
          {/if}
        </div>
        <div class="center-content">
          {#if properties.label}
            <div
              class="center-text"
              class:prevent-focus={preventFocus}
              on:click={handleCenterTextClick}
              on:keydown
              role="button"
              tabindex="0"
            >
              <!-- eslint-disable-next-line -->
              {@html properties.label}
            </div>
          {/if}
          {#if $$slots.centerContent}
            <slot name="centerContent" />
          {/if}
        </div>
        <div class="right-content">
          {#if $$slots.rightContent}
            <slot name="rightContent" />
          {/if}
          {#if properties.rightImageUrl}
            <div
              class:prevent-focus={preventFocus}
              on:click={handleRightImageClick}
              on:keydown
              role="button"
              tabindex="0"
            >
              <img class="right-img" src={properties.rightImageUrl} alt="" />
            </div>
          {/if}
          {#if showRightContentLoader}
            <Loader />
          {/if}
          {#if properties.rightContentText && properties.rightContentText !== ''}
            <span class="right-content-text">{properties.rightContentText}</span>
          {/if}
        </div>
      </div>
      <div class="bottom-section">
        {#if $$slots.bottomContent && properties.useAccordion}
          <Accordion bind:expand>
            <slot name="bottomContent" />
          </Accordion>
        {/if}
      </div>
    </div>
  </div>
{/if}

<style>
  .item-container {
    position: relative;
    --loader-foreground: var(--list-item-loader-foreground, #86898d);
    --loader-background: var(--list-item-loader-background, #ffffff);
    --loader-foreground-end: var(--list-item-loader-foreground-end, #ffffff);
  }

  .item-loader {
    position: absolute;
    height: 100%;
    width: 100%;
    background: var(--list-item-loader-background-color, #00000030);
    z-index: 20;
    animation: fill-loader ease-in-out var(--list-item-loader-duration, 8s);
  }

  @keyframes fill-loader {
    0% {
      width: 0;
    }

    100% {
      width: 100%;
    }
  }

  .item {
    display: flex;
    flex-direction: column;
    background-color: var(--list-item-background-color, transparent);
    -webkit-tap-highlight-color: transparent;
    cursor: var(--list-item-cursor, pointer);
    -moz-box-shadow: var(--list-item-box-shadow, none);
    -webkit-box-shadow: var(--list-item-box-shadow, none);
    box-shadow: var(--list-item-box-shadow, none);
    width: var(--list-item-box-width);
    border-radius: var(--list-item-border-radius, 0px);
    margin: var(--list-item-margin);
    padding: var(--list-item-padding);
    border: var(--list-item-border);
    transition: var(--list-item-transition);
  }

  .item:hover {
    background-color: var(--list-item-hover-background-color, var(--list-item-background-color));
    border: var(--list-item-hover-border, var(--list-item-border));
  }

  .top-section {
    display: flex;
    flex-direction: row;
    margin-bottom: 0;
  }

  .left-content {
    display: var(--list-item-left-content-display, flex);
    --image-height: var(--list-item-left-image-height, 24px);
    --image-width: var(--list-item-left-image-width, 24px);
    --image-padding: var(--list-item-left-image-padding, 0px);
    --image-border-radius: var(--list-item-left-image-border-radius, 0px);
    --image-margin: var(--list-item-left-image-margin, 0px);
    --image-filter: var(--list-item-left-image-filter, none);
    --image-background: var(--list-item-left-image-background);
    --image-border: var(--list-item-left-image-border);
    --image-transition: var(--list-item-transition);
    --image-object-fit: var(--list-item-left-image-object-fit);
    --image-hover-background: var(
      --list-item-left-image-hover-background,
      var(--list-item-left-image-background)
    );
    --image-hover-border: var(
      --list-item-left-image-hover-border,
      var(--list-item-left-image-border)
    );
  }

  .center-text {
    display: flex;
    flex-direction: column;
    justify-content: var(--list-item-center-text-justify-content, flex-start);
    padding: var(--list-item-center-text-padding, 0px 20px);
    color: var(--list-item-center-text-color, #2f3841);
    font-size: var(--list-item-center-text-font-size, 12px);
    font-weight: var(--list-item-center-text-font-weight, 300);
    align-items: var(--list-item-center-text-vertical-align);
    margin: var(--list-item-center-text-margin);
    border: var(--list-item-center-text-border);
    cursor: var(--list-item-center-text-cursor, pointer);
    font-family: var(--list-item-center-text-font-family);
  }

  .center-content {
    display: flex;
    flex: 1;
    min-width: 0;
  }

  .right-content {
    display: var(--list-item-right-content-display, flex);
  }

  .bottom-section {
    flex-direction: row;
    margin-top: 0;
  }

  .right-img {
    height: var(--list-item-right-image-height, 18px);
    width: var(--list-item-right-image-width, 18px);
    padding: var(--list-item-right-image-padding, 0px);
    border-radius: var(--list-item-right-image-border-radius, 0px);
    margin: var(--list-item-right-image-margin, 0px);
    filter: var(--list-item-right-image-filter);
    background: var(--list-item-right-image-background);
    border: var(--list-item-right-image-border);
    transition: var(--list-item-transition);
  }

  .right-img:hover {
    background: var(
      --list-item-right-image-hover-background,
      var(--list-item-right-image-background)
    );
    border: var(--list-item-right-image-hover-border, var(--list-item-right-image-border));
  }

  .right-content-text {
    color: var(--list-item-right-content-text-color, #2f3841);
    font-size: var(--list-item-right-content-text-font-size, 12px);
    font-weight: var(--list-item-right-content-text-font-weight, 300);
    display: var(--list-item-right-content-display, flex);
    align-items: var(--list-item-right-content-text-vertical-align);
    padding: var(--list-item-right-content-text-padding, 0px);
    margin: var(--list-item-right-content-text-margin, 0px);
    border: var(--list-item-right-content-text-border);
    cursor: var(--list-item-right-content-text-cursor, pointer);
    font-family: var(--list-item-right-content-text-font-family);
    justify-content: var(--list-item-right-content-text-justify-content);
  }

  .prevent-focus:focus {
    outline: none;
  }
</style>
