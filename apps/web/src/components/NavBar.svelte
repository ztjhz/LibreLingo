<script lang="ts">
import { stores } from '@sapper/app';
  import settings from "../settings"
  import authStore from "../auth"
  import NavBar from "lluis/NavBar/NavBar.svelte"
  import Icon from "lluis/Icon.svelte"
  import NavBarButton from "lluis/NavBar/NavBarButton.svelte"
  import Logo from "lluis/Logo.svelte"
  import NavBarItem from "lluis/NavBar/NavBarItem.svelte"
  import NavBarButtonSet from "lluis/NavBar/NavBarButtonSet.svelte"
  export let hasAuth = false
  export let repositoryURL = null

  type WindowWithLogout = Window & {
    _Logout: () => void
  }
  const _Logout = () => (window as unknown as WindowWithLogout)._Logout()

  const { page } = stores();
  
  const homepageLink = $page?.params?.courseName
    ? `course/${$page.params.courseName}/`
    : "/";
</script>

<NavBar data-test="navbar">
  <div slot="left">
    <Logo src="/images/logo.svg" alt="LibreLingo" link={homepageLink}/>
  </div>

  <div slot="right">
    <NavBarButtonSet>
      {#if repositoryURL}
        <NavBarButton href={repositoryURL} target="_blank">Feedback</NavBarButton>
      {/if}
      {#if hasAuth && settings.features.authEnabled}
        {#if $authStore.user}
          <NavBarItem>
            <Icon size="small" icon="user" />
            <span>{$authStore.user.name}</span>
          </NavBarItem>
          <NavBarButton on:click="{() => _Logout()}">Log out</NavBarButton>
        {:else}
          <NavBarButton href="/sign-up">Sign up</NavBarButton>
          <NavBarButton href="/login">Log in</NavBarButton>
        {/if}
      {/if}
    </NavBarButtonSet>
  </div>
</NavBar>

<style>
  div {
    height: 100%;
  }
</style>
