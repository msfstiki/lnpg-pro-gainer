<script lang="ts">
    import { page } from '$app/stores';
    import data from '$utils/site-data';
    import { serializeSchema } from '$utils/json-ld';
    import type { Schema } from '$utils/json-ld';
    import { browser } from '$app/env';
    export let schemas: Schema[];
    export let canonical = '';
    export let title: string;
    export let isPost = false;
    export let thumbnail = '';
    export let desc = '';
    export let noindex = false;
    const { siteName, siteUrl, fbAppId } = data;
    const defaultDesc = data.description;
  </script>
  
  <svelte:head>
    <title>{siteName} | {title}</title>
    <link
      rel="canonical"
      href={canonical ? siteUrl + canonical : siteUrl + ($page.url.pathname ?? '')} />
    <meta name="description" content={desc || defaultDesc} />
    <meta name="robots" content={noindex ? 'noindex' : 'all'} />
    <!-- Open Graph / Facebook -->
    <meta property="fb:app_id" content="{fbAppId}" />
    <meta property="og:type" content={isPost ? 'blog' : 'website'} />
    <meta property="og:url" content="{siteUrl}{$page.url.pathname ?? ''}" />
    <meta property="og:title" content={title || siteName} />
    <meta property="og:description" content={desc || defaultDesc} />
    <meta property="og:image" content={thumbnail || siteUrl + '/favicon.png'} />
    <!-- Twitter -->
    <meta property="twitter:card" content="summary_large_image" />
    <meta property="twitter:url" content="{siteUrl}{$page.url.pathname ?? ''}" />
    <meta property="twitter:title" content={title || siteName} />
    <meta property="twitter:description" content={desc || defaultDesc} />
    <meta name="msapplication-TileImage" content="/assets/site/logo-270x270.png">
    {#if thumbnail !== ''}<meta property="twitter:image" content={thumbnail} />{/if}
    {#if !browser}<!-- RSS Feed -->
      <link
        rel="alternate"
        type="application/rss+xml"
        title="RSS Feed for mailcheck.co"
        href="/rss.xml" />
  
      <!-- JSON-LD Schema -->
      {#each schemas as schema}
        {@html serializeSchema(schema)}
      {/each}{/if}
      
    <link rel="icon" href="/assets/site/logo-32x32.png" sizes="32x32">
    <link rel="icon" href="/assets/site/logo-192x192.png" sizes="192x192">
    <link rel="apple-touch-icon" href="/assets/site/logo-180x180.png">
  </svelte:head>