<script>
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import { MotionPathPlugin } from "gsap/MotionPathPlugin";

  gsap.registerPlugin(ScrollTrigger, MotionPathPlugin);

  onMount(() => {
    const path = document.querySelector("#line");
    const circle = document.querySelector("#movingCircle");

    if (path && circle) {
      gsap.to(circle, {
        scrollTrigger: {
          trigger: path,
          start: "top top",
          end: "bottom bottom",
          scrub: 1,
          pin: true
        },
        motionPath: {
          path: path,
          start: 0,
          end: 1,
          align: path,
          autoRotate: true,
          alignOrigin: [0.5, 0.5]
        }
      });

      // Beispiel für Trigger an bestimmten Punkten
      const triggerPoints = [
        { position: 0.25, teaserId: "teaser1" },
        { position: 0.5, teaserId: "teaser2" },
        { position: 0.75, teaserId: "teaser3" }
      ];

      triggerPoints.forEach((point) => {
        ScrollTrigger.create({
          trigger: path,
          start: `top top+=${point.position * path.getTotalLength()}`,
          end: `top top+=${(point.position + 0.1) * path.getTotalLength()}`, // Adjust the end position
          onEnter: () =>
            (document.getElementById(point.teaserId).style.display = "block"),
          onLeave: () =>
            (document.getElementById(point.teaserId).style.display = "none"),
          onLeaveBack: () =>
            (document.getElementById(point.teaserId).style.display = "none")
        });
      });
    } else {
      console.error("SVG path or circle not found");
    }
  });
</script>

<svg
  width="974"
  height="1891"
  viewBox="0 0 974 1891"
  fill="#53ABA4"
  xmlns="http://www.w3.org/2000/svg"
>
  <path
    id="line"
    d="M66 40L66 547.105C66.0565 650.334 149.758 734 253 734H747C850.277 734 934 817.723 934 921V1259C934 1352.89 857.441 1429 763 1429H556C462.112 1429 386 1352.44 386 1258V1067C386 972.559 309.441 896 215 896C120.559 896 44 972.559 44 1067L40.5 1686.5V1687.5C40.5 1781.94 120.559 1851 215 1851H640"
    stroke="#53ABA4"
    stroke-width="80"
    stroke-linecap="round"
  />
  <circle id="movingCircle" cx="10" cy="1679" r="40" fill="#2E908B" />
</svg>

<!-- Teaser Elemente -->
<div id="teaser1" class="teaser" style="display: none;">Teaser 1 Inhalt</div>
<div id="teaser2" class="teaser" style="display: none;">Teaser 2 Inhalt</div>
<div id="teaser3" class="teaser" style="display: none;">Teaser 3 Inhalt</div>

<style>
  .teaser {
    position: fixed;
    top: 20%;
    left: 50%;
    transform: translateX(-50%);
    background-color: white;
    padding: 20px;
    border: 1px solid #ccc;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
</style>
