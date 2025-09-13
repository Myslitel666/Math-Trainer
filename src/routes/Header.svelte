<script>
  import { goto } from "$app/navigation";
  import Header from "svelte-elegant/Header";
  import ColorThemeSwitch from "svelte-elegant/ColorThemeSwitch";
  import { DiagramIconPro, Calculator } from "svelte-elegant/icons-elegant";
  import { IconHover } from "svelte-elegant";

  import { themeMode, themeStore } from "svelte-elegant/stores";

  let theme;

  let logotypeFilter = "";
  let svelteColor = "";
  let elegantColor = "";

  // Подписываемся на изменения темы
  themeStore.subscribe((value) => {
    theme = value; //Инициализация объекта темы

    svelteColor = theme.palette.primary;
    elegantColor = $themeMode === "light" ? "#383838" : "#fdfdfd";
    logotypeFilter = $themeMode === "light" ? "brightness(80%)" : "";
  });
</script>

<Header>
  <button style:gap="0.5rem" onclick={() => goto("/settings")}>
    <span style:margin-left="-7px" style:margin-top="4px">
      <Calculator size="35px" />
    </span>
    <p style:font-size="26px" style:margin-left="-5px" style:margin-top="2px">
      <span
        style:color={svelteColor}
        style:filter={logotypeFilter}
        style:transition="all 0.3s"
      >
        Math
      </span>
      <span style:color={elegantColor} style:transition="all 0.3s">
        Trainer
      </span>
    </p>
  </button>
  <div class="icons">
    <button style:margin-top="4px" onclick={() => goto("/data")}>
      <DiagramIconPro size="49px" />
    </button>
    <ColorThemeSwitch />
  </div>
</Header>

<style>
  button {
    margin-left: 6px;
  }

  .icons {
    display: flex;
    align-items: center;
    gap: 6px;
    margin-left: auto;
    margin-right: 6px;
  }

  @media (max-width: 768px) {
    button {
      margin-left: 3px;
    }
  }
</style>
