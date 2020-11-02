<style>
  .scroll-hint {
    display: block;
    width: 32px;
    margin: 1em 0 2em;
    stroke-width: 2;
    stroke: white;
    fill: none;
    stroke-linecap: round;
    stroke-linejoin: round;
    animation: scroll-hint 10s linear infinite 5s;
  }

  @keyframes scroll-hint {
    9.999%, 19.999% {
      transform: translate(0, 5px);
    }
    10%, 20% {
      transform: translate(0, 0);
    }
  }
</style>

<svg xmlns="http://www.w3.org/2000/svg" class="scroll-hint" viewBox="0 0 24 24">
  <polyline points="7 7 12 12 17 7" />
  <polyline points="7 13 12 18 17 13" />
</svg>
