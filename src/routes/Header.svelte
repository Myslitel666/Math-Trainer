<script>
  import { goto } from "$app/navigation";
  import Header from "svelte-elegant/Header";
  import ColorThemeSwitch from "svelte-elegant/ColorThemeSwitch";
  import { DatabaseCheck } from "svelte-elegant/icons-elegant";

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
    <p style:font-size="26px">
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
    <button onclick={() => goto("/data")}>
      <DatabaseCheck size="37px" />
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
    gap: 5px;
    margin-left: auto;
    margin-right: 1px;
  }

  @media (max-width: 768px) {
    button {
      margin-left: 3px;
    }
  }
</style>
