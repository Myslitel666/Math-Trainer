<script>
  import { Button, AutoComplete, TextField } from "svelte-elegant";
  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  // Загружаем значение из localStorage или используем true по умолчанию
  let difficultyLevels = "";
  let trainingTime = "";

  let isInitialized = false;

  onMount(() => {
    difficultyLevels = "Medium";
    isInitialized = true;
    let storedTime = localStorage.getItem("time");

    if (storedTime) {
      trainingTime = storedTime;
    } else {
      trainingTime = "1";
    }
  });

  function navigate(link) {
    goto(link);
  }

  $: {
    if (Number(trainingTime) > 60) {
      trainingTime = "60";
    } else if (Number(trainingTime) < 0) {
      trainingTime = "1";
    }
  }
</script>

<div class="container">
  <div class="content">
    <p
      style:font-size="1.2rem"
      style:font-weight="500"
      style:margin-bottom="0.12rem"
    >
      Select Mode
    </p>
    <div class="switch-container">
      <p class="width">Time (min):</p>
      <div style:margin-left="0.5rem">
        <TextField
          label="Training time"
          type="number"
          width="12.5rem"
          bind:value={trainingTime}
        />
      </div>
    </div>
    <div class="switch-container">
      <p class="width">Difficulty Levels:</p>
      <div style:margin-left="0.5rem">
        <AutoComplete
          isSelect
          label="Memory Items"
          width="12.5rem"
          bind:value={difficultyLevels}
          options={["Easy", "Medium", "Hard"]}
        />
      </div>
    </div>
    <div class="mgn-top" style:width="100%">
      <Button
        width="100%"
        onClick={() => {
          localStorage.setItem("difficultyLevels", difficultyLevels);
          localStorage.setItem("time", trainingTime);
          navigate("/home");
        }}
      >
        Ready to Go!
      </Button>
    </div>
  </div>
</div>

<style>
  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
  }

  .content {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .width {
    width: 6.75rem;
  }

  .mgn-top {
    margin-top: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .switch-container {
    display: flex;
    align-items: center;
    margin-top: 0.77rem;
  }
</style>
