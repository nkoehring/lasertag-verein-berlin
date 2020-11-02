<script>
  import LogoHeader from '../components/LogoHeader.svelte'
  import SocialIcons from '../components/SocialIcons.svelte'
  import ScrollHint from '../components/ScrollHint.svelte'
</script>

<style>
</style>

<svelte:head>
  <title>Lasertag-Verein Berlin</title>
</svelte:head>

<div id="landing">
  <LogoHeader>
    <span slot="headline">Lasertag Verein Berlin</span>
    <SocialIcons />
    <ScrollHint />
  </LogoHeader>
  <main>
    <h1>Willkommen beim Lasertag Verein</h1>
  </main>
</div>
