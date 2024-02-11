<script>
  import "./app.css";
  import { browser } from "$app/environment";
  import confetti from "canvas-confetti";

  const stageMappings = {
    default: {
      text: "err... wat da hek? how u get here",
      yes: "u shouldin be here >:c",
      no: "STOP BREAKIMG MY CODE >:((((((((((",
    },
    "3": {
      text: "o myGUOBAAA YAAAYYYYY",
      yes: "HAPPY HAPPY HAPPY",
      no: "JUS KIBINGG!! i super duper trolled u LOSR",
    },
    "2": {
      text: "r u trolimg me...... >:c",
      yes: "nyuh uhhhhh... i woul nevr trol,,",
      no: "maybe.....ee",
    },
    "1": {
      text: "YAAAAYWYYWYAYWAYYAYYYY :33 REALY??",
      yes: "MHM!!! YAA ILL BE UR VALENTIB",
      no: "erm.... i chage my mind.. ur TUPID!!! :3",
    },
    "0": {
      text: "will u be my valentine 🥺",
      yes: "YAB :3 🤤",
      no: "nyo 😡 u STINK and ur STUPID 😡",
    },
    "-1": {
      text: "wah... realy? :(",
      yes: "nyooo im jus kidimg 😊",
      no: "yah ur a loser :3cn",
    },
    "-2": {
      text: "WYYYY :C",
      yes: "umm..  i piked da wrong thing :3",
      no: "cuz u SUK AND UR STUPID",
    },
    "-3": {
      text: "*kil you*",
      yes: "NYUHH IW AS JOKIMGG STOPPY AAH",
      no: "*blegh*",
    },
  };

  const endingMappings = {
    default: {
      text: "err... wat da hek? how u get here..<br />u shouldin be here >:c..<br />STOP BREAKIMG MY CODE >:((((((((((",
      emoji: ["🤤", browser ? confetti.shapeFromText("🤤") : null],
    },
    positive: {
      text: "YAYYYY I LOVE YOUUU <3333",
      emoji: ["🥰", browser ? confetti.shapeFromText("🥰") : null],
    },
    negative: {
      text: "u suk.",
      emoji: ["😡", browser ? confetti.shapeFromText("😡") : null],
    },
  };

  let isValentines = () => {
    const date = new Date();
    return date.getMonth() == 1 && date.getDate() === 14;
  };

  let max = Object.keys(stageMappings).length / 2 - 1;
  let debug = false;
  let ending = "none";
  let stage = 0;

  $: {
    if (stage > max) {
      stage = max;
      ending = "positive";
    } else if (stage < -max) {
      stage = -max;
      ending = "negative";
    }
  }

  $: {
    let animation = "";
    const random = Math.random();

    if (stage > 0) {
      if (random > 0.85) animation = "spin";
      else if (true || random > 0.7) animation = "bounce";
    } else {
      if (random > 0.85) animation = "intenseshake";
      else if (true || random > 0.7) animation = "shake";
    }

    if (animation && browser && stage !== 0) {
      const credits = document.querySelector("#info a");
      if (credits) {
        credits.style.opacity = 0;
        setTimeout(() => (credits.style.opacity = 1), 1000);
      }

      document.body.style.animation = `0.35s linear 2 ${animation}`;
    }
  }

  $: currentStage = stageMappings[String(stage)] || stageMappings["default"];
  $: currentEnding =
    endingMappings[String(ending)] || endingMappings["default"];

  const throttle = (fn, timeout) => {
    let activate = true;
    return () => {
      if (!activate) return;
      activate = false;
      fn.apply(this, arguments);
      setTimeout(() => (activate = true), timeout);
    };
  };

  if (browser) {
    document.onmousemove = throttle(
      () =>
        ending !== "none"
          ? confetti({
              shapes: Math.random() > 0.2 ? [currentEnding.emoji[1]] : null,
              scalar: Math.max(Math.random() * 3, 1),
              origin: { x: Math.random(), y: Math.random() },
              ticks: 125,
            })
          : null,
      100
    );
  }
</script>

{#if isValentines()}
  {#if ending === "none"}
    <h1>valentines game!! &lt;3</h1>
    <img id="image" src="stage{stage}.gif" alt="emotion" />
    <h1>{currentStage.text}</h1>
    <div id="answers">
      <button
        id="yes"
        on:click={() => stage++}
        style="flex-grow: {(Math.abs(stage) / max) * 2 + 1};"
        >{@html currentStage.yes}</button
      >
      <button
        id="no"
        on:click={() => stage--}
        style="opacity: {Math.min(Math.max((stage + max) / max, 0.1), 1)}"
        >{@html currentStage.no}</button
      >
    </div>
  {:else}
    <h1>
      {ending === "none" ? "INYUNYO" : ending === "negative" ? "BAD" : "GOOD"} ENDING{ending ===
      "negative"
        ? "."
        : "!"}
    </h1>
    <img src="ending-{ending}.gif" alt="{ending} ending" />
    <span>{@html currentEnding.text}</span>
  {/if}
{:else}
  <img id="image" src="goaway.gif" alt="GO AWAY!!! LOSER!!" />

  <h1>
    erm... are u tupid???<br />
    is it NYOT VALENTINES DAY.....<br />
    GO AWAY!! &gt;:c
  </h1>
  <button
    on:click={() => location.replace("https://github.com/celestialcrafter")}
    >GO AWAY!!</button
  >
{/if}

<div id="info">
  {#if debug}
    <div id="debug">
      <span
        >(debug) stage: <span
          style="color: {stage == 0
            ? 'var(--ctp-mocha-subtext0)'
            : stage > 0
              ? 'var(--ctp-mocha-green)'
              : 'var(--ctp-mocha-red)'}">{stage}</span
        ></span
      ><br />
      <span>(debug) ending: {ending}</span>
    </div>
  {/if}

  <a
    href="https://github.com/celestialcrafter"
    on:contextmenu={(event) => {
      event.preventDefault();
      debug = !debug;
    }}
    on:drag={() => {
      isValentines = () => true;
      alert("valentines bypass activated");
    }}>made by celestial ❤️</a
  >
</div>
