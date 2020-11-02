<script>
  import BackgroundImage from 'images/logo.jpg'
</script>

<style>
#logo-header {
  display: flex;
  flex-flow: column nowrap;
  width: 100vw;
  height: 100vh;
  justify-content: center;
  align-items: center;
}

#logo-header > .logo {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  max-width: 100vw;
  max-height: 100vh;
  width: 800px;
  height: 800px;
  margin: auto;
  background: black center no-repeat;
}

#logo-header > .logo > .headline {
  margin: .5em;
  font: italic 3.0em "norwester", "Impact", Helvetica, sans-serif;
  text-align: right;
  text-shadow: 2px 2px black;
}

@media (max-width: 500px) {
  #logo-header > .logo > .headline {
    width: 220px;
  }
}
</style>

<header id="logo-header">
  <div class="logo" style="background-image: url({BackgroundImage})">
    <div class="headline"><slot name="headline" /></div>
  </div>
  <slot />
</header>
