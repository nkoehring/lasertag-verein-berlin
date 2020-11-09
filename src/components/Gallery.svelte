<script>
  export let items = []
  export let currentIndex = 0

  let nextCurrent = currentIndex
  let transitionToNextImage = false
  let transitionToPrevImage = false

  const gesturing = {
    active: false,    // activates move event
    cancelled: false, // enables transitioning back to original state
    threshholdX: 350, // pixels to move to trigger image switch
    threshholdT: 200, // max milliseconds to trigger immediate switch
    startX: 0,        // we're only interested in the x-axis
    startTime: 0,     // to detect fast swipes
    deltaX: 0,        // used to calculate shift on touchMove
  }

  $: lastItemIndex = items.length - 1
  $: prevItemIndex = currentIndex > 0 ? currentIndex - 1 : lastItemIndex
  $: nextItemIndex = currentIndex < lastItemIndex ? currentIndex + 1 : 0

  $: currentItems = [
    items[prevItemIndex],
    items[currentIndex],
    items[nextItemIndex]
  ]

  $: cssVars = `
    --shift-left: ${gesturing.deltaX < 0 ? gesturing.deltaX : 0}px;
    --shift-right: ${gesturing.deltaX > 0 ? gesturing.deltaX : 0}px;
    --transition-time: ${gesturing.cancelled ? 300 : 0}ms;
  `

  function prevItem () {
    transitionToPrevImage = true
    nextCurrent = prevItemIndex
  }

  function nextItem () {
    transitionToNextImage = true
    nextCurrent = nextItemIndex
  }

  function transitionEnd () {
    currentIndex = nextCurrent
    transitionToNextImage = false
    transitionToPrevImage = false
    gesturing.cancelled = false
  }

  function handleTouchStart (event) {
    event.stopImmediatePropagation()
    event.stopPropagation()
    gesturing.active = true
    gesturing.cancelled = false
    gesturing.startX = event.x
    gesturing.startTime = event.timeStamp
  }
  function handleTouchEnd (event) {
    event.stopImmediatePropagation()
    event.stopPropagation()
    gesturing.active = false
    gesturing.deltaX = 0

    const deltaX = event.x - gesturing.startX
    const deltaT = event.timeStamp - gesturing.startTime
    const { threshholdT, threshholdX } = gesturing
    const isFast = Math.abs(deltaX) > 10 && deltaT < threshholdT
    const isFar = Math.abs(deltaX) > threshholdX
    gesturing.cancelled = !isFast && !isFar

    if (!gesturing.cancelled) {
      if (deltaX < 0) nextItem()
      else prevItem()
    }
  }
  function handleTouchMove (event) {
    if (!gesturing.active) return
    event.stopImmediatePropagation()
    event.stopPropagation()
    gesturing.deltaX = event.x - gesturing.startX
  }
</script>

<style>
.gallery-strip {
  display: flex;
  flex-flow: row nowrap;
  width: 100%;
}

.image-container {
  flex: 0 0 auto;
  justify-content: center;
  width: 80%;
  margin: 0;
  display: flex;
  flex-flow: row nowrap;
  overflow: hidden;
}
.image-container > img {
  flex: 0 0 auto;
  width: 100%;
  pointer-events: none;
  transition: transform var(--transition-time) ease-in;
}
.image-container > img.prev {
  transform: translate(var(--shift-right));
}
.image-container > img.next {
  transform: translate(var(--shift-left));
}

.image-container.prev > img.prev ,
.image-container.next > img.next {
  transition: transform .4s ease-in-out;
}
.image-container.prev > img.prev {
  transform: translate(100%);
}
.image-container.next > img.next {
  transform: translate(-100%);
}

.gallery-action {
  flex: 0 0 auto;
  width: 10%;
  border: none;
  background-color: transparent;
  font-size: 2em;
  color: #CCD;
  cursor: pointer;
  transition: background-color .3s ease-in;
}
.gallery-action:hover {
  color: #FFF;
  background-color: #333;
}
</style>

<div class="gallery-strip" style={cssVars}>
  <button class="gallery-action prev" on:click={prevItem}> ❰ </button>
  <div class="image-container"
    class:next={transitionToNextImage}
    class:prev={transitionToPrevImage}
    on:pointerdown={handleTouchStart}
    on:pointerup={handleTouchEnd}
    on:pointermove={handleTouchMove}
    on:transitionend={transitionEnd}
  >
    {#each currentItems as {src, alt}, i (src)}
    <img
      class:prev={i === 0}
      class:next={i === 2}
      {src}
      {alt}
    />
    {/each}
  </div>
  <button class="gallery-action next" on:click={nextItem}> ❱ </button>
</div>
