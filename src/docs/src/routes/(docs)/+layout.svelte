<script>
  import { onMount } from "svelte"
  import { pagesThatDontNeedSidebar } from "@src/lib/data.js"

  import { currentLang, langs } from "@src/lib/i18n"

  onMount(() => {
    let lang = new URL(document.location).searchParams.get("lang")
    if (langs.includes(lang)) {
      $currentLang = lang
      localStorage.setItem("lang", $currentLang)
    }
    if (localStorage.getItem("lang")) {
      $currentLang = localStorage.getItem("lang")
    }
  })

  import { page } from "$app/stores"

  import "@components/StyleHandler.svelte"

  import "prism-themes/themes/prism-material-dark.min.css"
  import "@src/prism-themes-modify.css"

  import Navbar from "@components/Navbar.svelte"
  import Scripts from "@components/Scripts.svelte"

  import Sidebar from "@components/Sidebar.svelte"

  // let Sidebar
  // const sleep = (ms) => new Promise((f) => setTimeout(f, ms))
  // onMount(async () => {
  //   await sleep(1000)
  //   Sidebar = (await import("../../components/Sidebar.svelte")).default
  // })

  let drawersidebar
  let drawerSidebarScrollY = 0
  function parseSidebarScroll() {
    drawerSidebarScrollY = drawersidebar.scrollTop
  }
  onMount(() => {
    parseSidebarScroll()
    document.documentElement.style.scrollPaddingTop = "5rem"
    document.documentElement.style.scrollBehavior = "smooth"
  })

  let checked = ""
  function closeDrawer() {
    checked = ""
  }

  function openDrawer() {
    checked = true
  }

  let navbarScrollPadding = "5rem"
  function addScrollPaddingToNavbar(action) {
    navbarScrollPadding = "5rem"
    document.documentElement.style.scrollPaddingTop = "5rem"
  }

  function removeScrollPaddingFromNavbar(action) {
    navbarScrollPadding = "0rem"
    document.documentElement.style.scrollPaddingTop = "0rem"
  }
</script>

<svelte:head>
  <link
    media="screen and (min-width: 520px)"
    rel="preconnect"
    href="https://fonts.googleapis.com" />
  <link
    media="screen and (min-width: 520px)"
    rel="preconnect"
    href="https://fonts.gstatic.com"
    crossorigin />
  <link
    media="screen and (min-width: 520px)"
    href="https://fonts.googleapis.com/css2?family=Figtree:wght@300;900&family=Noto+Sans+JP:wght@300;900&family=Noto+Sans:wght@300;900&display=swap"
    rel="stylesheet" />
</svelte:head>

<div
  class={`bg-base-100 drawer ${
    pagesThatDontNeedSidebar.includes($page.url.pathname) ? "" : "lg:drawer-open"
  }`}>
  <input id="drawer" type="checkbox" class="drawer-toggle" bind:checked />
  <div class={`drawer-content`}>
    <Navbar
      {addScrollPaddingToNavbar}
      {removeScrollPaddingFromNavbar}
      showBlogBtn="true"
      showComponentsBtn="true"
      hideLogoOnLargeScreen={$page.url.pathname == "/" ? false : true}
      hideSidebarButtonOnLargeScreen={$page.url.pathname == "/" ? false : true}
      showSearch="true"
      showVersion="true"
      showLanguage="true" />
    <div
      class={`${
        pagesThatDontNeedSidebar.includes($page.url.pathname)
          ? ""
          : "max-w-[100vw] px-6 pb-16 xl:pr-2"
      }`}>
      <slot />
    </div>
  </div>
  <div
    class="drawer-side z-40"
    style="scroll-behavior: smooth; scroll-padding-top: {navbarScrollPadding};"
    bind:this={drawersidebar}
    on:scroll={parseSidebarScroll}>
    <label for="drawer" class="drawer-overlay" aria-label="Close menu" />
    <aside class="bg-base-100 w-80">
      <svelte:component this={Sidebar} {closeDrawer} {openDrawer} {drawerSidebarScrollY} />
      <div
        class="bg-base-100 pointer-events-none sticky bottom-0 flex h-40 [mask-image:linear-gradient(transparent,#000000)]" />
    </aside>
  </div>
</div>

{#if $page.url.pathname == "/components/modal/"}
  <input type="checkbox" id="my_modal_6" class="modal-toggle" aria-label="Open or close modal" />
  <div class="modal">
    <div class="modal-box">
      <h3 class="text-lg font-bold">Hello!</h3>
      <p class="py-4">This modal works with a hidden checkbox!</p>
      <div class="modal-action">
        <label for="my_modal_6" class="btn">Close</label>
      </div>
    </div>
  </div>

  <input type="checkbox" id="my_modal_7" class="modal-toggle" aria-label="Open or close modal" />
  <div class="modal">
    <div class="modal-box">
      <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
      <p class="py-4">
        You've been selected for a chance to get one year of subscription to use Wikipedia for free!
      </p>
    </div>
    <label class="modal-backdrop" for="my_modal_7">Close</label>
  </div>

  <div class="modal" id="my_modal_8">
    <div class="modal-box">
      <h3 class="text-lg font-bold">Hello!</h3>
      <p class="py-4">This modal works with anchor links</p>
      <div class="modal-action">
        <!-- svelte-ignore a11y-invalid-attribute -->
        <a href="#" class="btn" rel="external">Close</a>
      </div>
    </div>
  </div>
{/if}

<Scripts />
