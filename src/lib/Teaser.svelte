<script>
  export let headline = "";
  export let copy = "";
  export let cta = "";
</script>

<div class="teaser-content">
  <div class="headline">{headline}</div>
  <div class="copy">{copy}</div>
  <div class="cta">{cta}</div>
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
    color: #fff;
    position: absolute;
    top: 20px; /* Beispielposition - anpassen, um im sichtbaren Bereich zu sein */
    left: 20px; /* Beispielposition - anpassen, um im sichtbaren Bereich zu sein */
    z-index: 1000;
    pointer-events: none;
    transition: opacity 0.5s ease; /* Hinzufügen von Übergängen */
  }

  .headline {
    color: #fff;
    font-family: "Helvetica", sans-serif;
    font-size: 24px;
    font-style: normal;
    font-weight: 700;
    line-height: 32px;
    letter-spacing: -0.24px;
  }

  .copy {
    color: #fff;
    font-family: "Helvetica", sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    line-height: 150%;
  }

  .cta {
    color: #fff;
    font-family: "Bosch Sans", sans-serif;
    font-size: 14px;
    font-style: normal;
    font-weight: 700;
    line-height: 150%;
    letter-spacing: 0.56px;
    text-transform: uppercase;
  }
</style>
