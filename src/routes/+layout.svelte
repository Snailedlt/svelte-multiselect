<script lang="ts">
  import { page } from '$app/stores'
  import GitHubCorner from 'svelte-github-corner'
  import '../app.css'
  import { demo_routes } from './+layout'
  import { name, repository } from '../../package.json'

  $: is_current = (path: string) => {
    if (path === $page.url.pathname) return `page`
    if (path !== `/` && $page.url.pathname.includes(path)) return `page`
    return undefined
  }
</script>

<svelte:head>
  <base href={import.meta.env.CI ? `/${name}/` : ``} />
</svelte:head>

<GitHubCorner href={repository} />

{#if demo_routes.some((route) => $page.url.pathname.endsWith(route))}
  <a href="." aria-label="Back to index page">&laquo; back</a>
  <h1>
    <img src="favicon.svg" alt="Svelte MultiSelect" height="50" width="50" />&ensp;Svelte
    MultiSelect Examples
  </h1>

  <nav>
    {#each demo_routes as route, idx}
      {#if idx > 0}<strong>&bull;</strong>{/if}
      <a href={route} aria-current={is_current(route)}>{route}</a>
    {/each}
  </nav>

  <main>
    <slot />
  </main>
{:else}
  <!-- handles non-svx routes index.svelte and +error.svelte -->
  <slot />
{/if}

<style>
  h1 {
    text-align: center;
    display: flex;
    place-content: center;
    place-items: center;
  }
  a[href='.'] {
    font-size: 16pt;
    position: absolute;
    top: 2em;
    left: 2em;
    background-color: rgba(255, 255, 255, 0.1);
    padding: 1pt 5pt;
    border-radius: 3pt;
    transition: 0.2s;
  }
  a[href='.']:hover {
    background-color: rgba(255, 255, 255, 0.2);
  }
  nav {
    display: flex;
    gap: 1em 1ex;
    place-content: center;
    margin: 1em auto 3em;
    max-width: 45em;
    flex-wrap: wrap;
  }
  nav > a {
    padding: 0 4pt;
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 3pt;
    transition: 0.2s;
  }
  nav > a[aria-current='page'] {
    color: mediumseagreen;
  }
</style>
