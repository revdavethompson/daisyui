<script>
  import SEO from "@components/SEO.svelte"
  export let data
  let { posts } = data
</script>

<SEO
  title="daisyUI Blog"
  desc="daisyUI blog - Updates, ideas and resources"
  img="/images/blog.jpg" />
{#if posts.length === 0}
  <div
    class="border-base-content/10 rounded-box flex h-96 w-full grow flex-col items-center justify-center border">
    <h1 class="text-base-content/30">Coming soon</h1>
  </div>
{/if}
<div class="grid justify-items-stretch gap-6">
  {#each posts as post}
    <a
      class="card sm:card-side hover:bg-base-200 transition-colors sm:max-w-none"
      href={`/blog/${post.slug}`}>
      {#if post.thumbnail}
        <figure class="mx-auto w-full object-cover p-6 max-sm:pb-0 sm:max-w-[12rem] sm:pr-0">
          <img
            loading="lazy"
            src={post.thumbnail}
            class="border-base-content bg-base-300 rounded-btn border border-opacity-5"
            alt={post.title}
            style={`view-transition-name: ${encodeURI(post.slug).replace(/[^a-zA-Z]/g, "")}-img`} />
        </figure>
      {/if}
      <div
        class="card-body"
        style={`view-transition-name: ${encodeURI(post.slug).replace(/[^a-zA-Z]/g, "")}-text`}>
        <h2 class="card-title">
          {post.title}
        </h2>
        <p class="text-xs opacity-60">
          {post.desc}
        </p>
      </div>
    </a>
  {/each}
</div>
