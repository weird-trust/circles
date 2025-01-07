<script>
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import { MotionPathPlugin } from "gsap/MotionPathPlugin";

  gsap.registerPlugin(ScrollTrigger, MotionPathPlugin);

  onMount(() => {
    const path = document.querySelector("#line");
    const circle = document.querySelector("#movingCircle");
    const teaserContent = document.querySelector("#teaserContent");
    const headline = teaserContent.querySelector(".teaser-headline");
    const copy = teaserContent.querySelector(".teaser-copy");
    const link = teaserContent.querySelector(".teaser-link");

    if (path && circle) {
      const pathLength = path.getTotalLength();

      const teaserPoints = [
        {
          progress: 0.2,
          headline: "Entwicklungen Automobilmarkt",
          copy: "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.Ut enim.",
          linkText: "Mehr erfahren →",
          linkHref: "#link1"
        },
        {
          progress: 0.4,
          headline: "Headline 2",
          copy: "Dies ist ein anderer erster Satz.<br>Ein weiterer Satz.<br>Noch ein Satz.",
          linkText: "Details →",
          linkHref: "#link2"
        },
        {
          progress: 0.5,
          headline: "Headline 3",
          copy: "Ein neuer erster Satz.<br>Ein neuer zweiter Satz.<br>Ein neuer dritter Satz.",
          linkText: "Weiterlesen →",
          linkHref: "#link3"
        }
      ];

      const animation = gsap.to(circle, {
        scrollTrigger: {
          trigger: path,
          start: "top top",
          end: "bottom bottom",
          scrub: 1,
          pin: true,
          onUpdate: (self) => {
            const progress = self.progress;
            updateTeaser(progress);
          }
        },
        motionPath: {
          path: path,
          align: path,
          autoRotate: true,
          alignOrigin: [0.5, 0.5]
        }
      });

      gsap.ticker.add(() => {
        const circlePosition = circle.getBoundingClientRect();
        teaserContent.style.transform = `translate(${circlePosition.x}px, ${circlePosition.y}px)`;
      });

      function updateTeaser(progress) {
        let teaserDisplayed = false;
        teaserPoints.forEach((point) => {
          if (Math.abs(progress - point.progress) < 0.2) {
            headline.textContent = point.headline;
            copy.innerHTML = point.copy;
            link.textContent = point.linkText;
            link.href = point.linkHref;
            gsap.to(teaserContent, {
              duration: 0.3,
              opacity: 1
            });
            teaserDisplayed = true;
          }
        });

        if (!teaserDisplayed) {
          gsap.to(teaserContent, {
            duration: 0.3,
            opacity: 0
          });
        }
      }
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
  <g id="movingCircle">
    <circle cx="10" cy="1679" r="40" fill="#2E908B" />
  </g>
</svg>

<div id="teaserContent" class="teaser-content">
  <h2 class="teaser-headline">Headline</h2>
  <p class="teaser-copy">
    Dies ist der erste Satz des Textes.<br />
    Dies ist der zweite Satz des Textes.<br />
    Dies ist der dritte Satz des Textes.
  </p>
  <a href="#" class="teaser-link">
    Mehr erfahren <span class="arrow">→</span>
  </a>
</div>

<style>
  .teaser-content {
    display: flex;
    width: 376px;
    height: auto;
    padding: var(--l, 32px);
    flex-direction: column;
    align-items: flex-start;
    gap: var(--m, 16px);
    align-self: stretch;
    border-radius: var(--m, 16px);
    background: var(--primary-bosch-trkis-50-markt-und-trends, #18837e);
    color: #fff;
    text-align: left;
    opacity: 0;
    height: auto;
    pointer-events: none;
    position: absolute; /* Use absolute positioning */
    z-index: 1000;
    transition: opacity 0.3s ease-in-out;
  }

  .teaser-headline {
    font-size: 24px;
    font-weight: bold;
    line-height: 32px;
    letter-spacing: -0.24px;
    margin: 0;
  }

  .teaser-copy {
    font-size: 14px;
    margin: 0;
    line-height: 1.5;
  }

  .teaser-link {
    font-size: 14px;
    font-weight: bold;
    text-transform: uppercase;
    color: #fff;
    text-decoration: none;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .teaser-link .arrow {
    font-size: 18px;
    transition: transform 0.2s;
  }

  .teaser-link:hover .arrow {
    transform: translateX(5px);
  }
</style>
