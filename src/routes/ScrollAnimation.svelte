<script>
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ScrollTrigger } from "gsap/ScrollTrigger";
  import { MotionPathPlugin } from "gsap/MotionPathPlugin";

  gsap.registerPlugin(ScrollTrigger, MotionPathPlugin);

  onMount(() => {
    const path = document.querySelector("#line");
    const circle = document.querySelector("#movingCircle circle");
    const teaserContent = document.querySelector("#teaserContent");

    if (path && circle && teaserContent) {
      const pathLength = path.getTotalLength();
      const startPoint = path.getPointAtLength(0);

      // Set the initial position of the circle to the start of the path
      gsap.set(circle, {
        x: startPoint.x,
        y: startPoint.y
      });

      // Define the positions of the teaser dots as a percentage of the total path length
      const teaserDots = [
        {
          position: 0.3,
          headline: "Teaser 1 Headline",
          copy: "Teaser 1 Inhalt",
          cta: "CTA Text Einfügen ->"
        }
        // {
        //   position: 0.5,
        //   headline: "Teaser 2 Headline",
        //   copy: "Teaser 2 Inhalt",
        //   cta: "Mehr erfahren ->"
        // },
        // {
        //   position: 0.7,
        //   headline: "Teaser 3 Headline",
        //   copy: "Teaser 3 Inhalt",
        //   cta: "Jetzt entdecken ->"
        // }
      ];

      // Animate the Userdot along the path
      gsap.to(circle, {
        scrollTrigger: {
          trigger: path,
          start: "top top",
          end: "bottom",
          scrub: true,
          pin: false
        },
        motionPath: {
          path: path,
          align: path,
          autoRotate: false,
          alignOrigin: [0.5, 0.5]
        }
      });

      teaserDots.forEach((dot) => {
        const dotPosition = path.getPointAtLength(dot.position * pathLength);

        // Create a teaser dot in the SVG
        const teaserdot = document.createElementNS(
          "http://www.w3.org/2000/svg",
          "circle"
        );
        teaserdot.setAttribute("cx", dotPosition.x);
        teaserdot.setAttribute("cy", dotPosition.y);
        teaserdot.setAttribute("r", 40);
        teaserdot.setAttribute("fill", "#2E908B");
        path.parentNode.appendChild(teaserdot);

        // Create a ScrollTrigger for each teaser dot
        ScrollTrigger.create({
          trigger: teaserdot,
          start: "top center",
          end: "bottom center",
          onEnter: () => {
            teaserContent.style.top = `${dotPosition.y}px`;
            teaserContent.style.left = `${dotPosition.x}px`;
            teaserContent.querySelector(".headline").textContent = dot.headline;
            teaserContent.querySelector(".copy").textContent = dot.copy;
            teaserContent.querySelector(".cta").textContent = dot.cta;

            gsap.fromTo(
              teaserContent,
              { opacity: 1, scale: 1 },
              {
                opacity: 1,
                scale: 1,
                duration: 0.75
              }
            );
          },
          onLeave: () => {
            gsap.to(teaserContent, { opacity: 1, duration: 0.75, scale: 1 });
          },
          onEnterBack: () => {
            teaserContent.style.top = `${dotPosition.y}px`;
            teaserContent.style.left = `${dotPosition.x}px`;
            teaserContent.querySelector(".headline").textContent = dot.headline;
            teaserContent.querySelector(".copy").textContent = dot.copy;
            teaserContent.querySelector(".cta").textContent = dot.cta;

            gsap.fromTo(
              teaserContent,
              { opacity: 1, scale: 1 },
              {
                opacity: 1,
                scale: 1,
                duration: 0.75
              }
            );
          },
          onLeaveBack: () => {
            gsap.to(teaserContent, { opacity: 0, duration: 0.75 });
          }
        });
      });
    } else {
      console.error("SVG path, circle, or teaser content not found");
    }
  });
</script>

<svg
  width="989"
  height="3469"
  viewBox="0 0 989 3469"
  fill="none"
  xmlns="http://www.w3.org/2000/svg"
>
  <path
    id="line"
    d="M504 40C504 98.72 551.78 146.5 610.5 146.5H753.5C806.71 146.5 850 189.34 850 242C850 294.66 806.71 337.5 753.5 337.5H246.5C132.64 337.5 40 429.69 40 543C40 656.31 132.64 750.5 246.5 750.5H510.5C587.42 750.5 650 813.08 650 890V1454C650 1531.47 587.42 1594.5 510.5 1594.5H246.5C209.59 1594.5 174.87 1608.95 148.73 1635.19C122.6 1661.42 108.13 1696.34 108 1733.5C108 1814 168.684 1872.5 248.5 1872.5H757.5C833.87 1872.5 896 1934.63 896 2011C896 2087.37 833.87 2149.5 757.5 2149.5H709.5C644.71 2149.5 592 2202.66 592 2268C591.46 2333.43 538.06 2386.5 472.5 2386.5H228.5C128.97 2386.5 48 2467.47 48 2567C48 2666.53 128.97 2747.5 228.5 2747.5H810.5C886.87 2747.5 949 2809.63 949 2886C949 2962.37 886.87 3024.5 810.5 3024.5H229.5C147.07 3024.5 80 3091.12 80 3173C80 3254.88 147.07 3321.5 229.5 3321.5L372 3321.52H396.5C454.85 3322.59 502 3370.4 502 3429"
    stroke="##116864"
    stroke-width="80"
    stroke-linecap="round"
  />
  <g id="movingCircle">
    <circle cx="0" cy="0" r="40" fill="#2E908B" />
  </g>
</svg>

<div id="teaserContent" class="teaser-content">
  <div class="headline"></div>
  <div class="copy"></div>
  <div class="cta"></div>
</div>

<style>
  .teaser-content {
    display: flex;
    padding: var(--l, 32px);
    flex-direction: column;
    align-items: flex-start;
    gap: var(--m, 16px);
    align-self: stretch;
    border-radius: var(--m, 16px);
    background: var(--primary-bosch-trkis-50-markt-und-trends, #18837e);
    opacity: 0;
    position: absolute;
    transform: translate(-50%, -50%);
    z-index: 1000;
    pointer-events: none;
    color: #fff;
    transform-origin: center center;
  }

  .headline {
    color: #fff;
    font-family: "Helvetica", sans-serif;
    font-size: 24px;
    font-style: normal;
    font-weight: 700;
    line-height: 32px; /* 133.333% */
    letter-spacing: -0.24px;
  }

  .copy {
    color: #fff;
    font-family: "Helvetica", sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    line-height: 150%; /* 21px */
  }

  .cta {
    color: #fff;
    font-family: "Helvetica", sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 700;
    line-height: 150%; /* 21px */
    letter-spacing: 0.56px;
    text-transform: uppercase;
  }
</style>
