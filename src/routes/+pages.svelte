<script>
  import ScrollAnimation from "./ScrollAnimation.svelte";
</script>

<main>
  <svg
    width="974"
    height="1891"
    viewBox="0 0 974 1891"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
  >
    <path
      d="M66 40L66 547.105C66.0565 650.334 149.758 734 253 734H747C850.277 734 934 817.723 934 921V1259C934 1352.89 857.441 1429 763 1429H556C462.112 1429 386 1352.44 386 1258V1067C386 972.559 309.441 896 215 896C120.559 896 44 972.559 44 1067L40.5 1686.5V1687.5C40.5 1781.94 120.559 1851 215 1851H640"
      stroke="#116864"
      stroke-width="80"
      stroke-linecap="round"
    />
  </svg>
  <ScrollAnimation />
</main>

<style>
  main {
    overflow: hidden;
    width: 100vw;
  }
</style>
