<script lang="ts">
  import { themeStore, themeMode } from "svelte-elegant/stores";
  import { onMount } from "svelte";
  import ButtonBox from "svelte-elegant/ButtonBox";

  let exampleColor = "";
  let firstNumber = 0;
  let secondNumber = 0;
  let memoryItems = "Numbers and Letters"; // значение по умолчанию
  let isInitialized = false;

  let inputStr = "";
  let isError = 0;
  let record = 0;
  let rightColor = "";
  let errColor = "";
  let num = "";
  let textRender = "";

  let operation = "";
  let operations = "+-•÷";

  let buttons = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
    ["Bs", 0, "En"],
  ];

  let theme: any;

  // Подписываемся на изменения темы
  themeStore.subscribe((value) => {
    theme = value; //Инициализация объекта темы

    if ($themeMode === "light") {
      rightColor = theme.palette.primary;
      errColor = "red";
    } else {
      rightColor = "#24F048";
      errColor = theme.palette.primary;
    }
  });

  function checkResult() {
    let inputAbs = Math.abs(Number(inputStr));
    let numAbs = Math.abs(Number(num));

    if (inputAbs === numAbs) {
      isError = 0;
    } else {
      isError = 1;
    }
    inputStr = "";

    genExample();
  }

  function genOperation() {
    let operationInd = Math.floor(Math.random() * operations.length);
    return operations[operationInd];
  }

  function genComplexNumber(a: number, b: number) {
    let isNotComplex = true;
    let num = 0;

    while (isNotComplex) {
      num = Math.floor(Math.random() * b) + a;

      if (num % 10 !== 0) {
        isNotComplex = false;
      }
    }

    return num;
  }

  function genExample() {
    operation = genOperation();

    //operation = "÷";

    if (operation === "+" || operation === "-") {
      firstNumber = genComplexNumber(11, 99);
      secondNumber = genComplexNumber(11, 99);

      if (operation === "+") {
        num = (firstNumber + secondNumber).toString();
      } else {
        num = (firstNumber - secondNumber).toString();
      }
    } else if (operation === "•") {
      firstNumber = genComplexNumber(11, 99);
      secondNumber = genComplexNumber(2, 9);
      num = (firstNumber * secondNumber).toString();
    } else {
      let quotient = genComplexNumber(11, 99);
      let divisor = genComplexNumber(2, 9);
      let dividend = quotient * divisor;

      firstNumber = dividend;
      let a = Math.floor(Math.random() * 2);

      if (a === 0) {
        secondNumber = divisor;
        num = (dividend / divisor).toString();
      } else {
        secondNumber = quotient;
        num = (dividend / quotient).toString();
      }
    }
  }

  onMount(() => {
    const storedValue = localStorage.getItem("memoryItems");

    if (storedValue) {
      memoryItems = storedValue;
    }
    isInitialized = true;

    genExample();
  });

  $: {
    if (inputStr.length > num.length) onBackClick();

    inputStr = inputStr.toLocaleUpperCase();

    textRender = inputStr;
    // Добавляем маскирующие символы
    const dotsToAdd = Math.max(0, num.length - textRender.length);
    textRender += "•".repeat(dotsToAdd);

    exampleColor = isError ? errColor : rightColor;

    console.log(num);
  }
  function onNumbClick(event: MouseEvent, button: string | number) {
    if (textRender !== num) {
      if (inputStr.length < num.length) {
        inputStr += button;
      }
    }
    //(event.target as HTMLElement).blur();
  }

  function onBackClick() {
    inputStr = inputStr.slice(0, -1);
  }

  function onEnterClick() {
    checkResult();
  }
</script>

{#if isInitialized}
  <div class="content">
    <div style:min-height="3rem" style:margin-top="0.35rem">
      <p class="render">
        <span>{firstNumber}</span>
        <span class="number" style:color={exampleColor}>{operation}</span>
        <span>{secondNumber}</span>
        <span class="number" style:color={exampleColor}>=</span>
        {#if secondNumber > firstNumber && operation === "-"}
          <span class="number" style:color={exampleColor}> - </span>
        {/if}
        <span style:color={exampleColor}>{textRender}</span>
      </p>
    </div>
    <div class="mgn-top">
      <div style:display="flex" style:flex-direction="column">
        {#each buttons as buttonLine}
          <div style:display="flex" style:flex-direction="row">
            {#each buttonLine as button}
              {#if button != "Bs" && button != "En"}
                <ButtonBox
                  marginRight={Number(button) % 3 === 0 && Number(button) !== 0
                    ? ""
                    : "0.75rem"}
                  marginBottom={button === "0" ? "" : "0.75rem"}
                  onClick={(event: MouseEvent) => {
                    onNumbClick(event, button);
                  }}
                >
                  {button}
                </ButtonBox>
              {:else if button == "Bs"}
                <ButtonBox
                  marginRight="0.75rem"
                  isPrimary={true}
                  onClick={onBackClick}
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                    class="size-6"
                    height="4rem"
                    width="4rem"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M2.515 10.674a1.875 1.875 0 0 0 0 2.652L8.89 19.7c.352.351.829.549 1.326.549H19.5a3 3 0 0 0 3-3V6.75a3 3 0 0 0-3-3h-9.284c-.497 0-.974.198-1.326.55l-6.375 6.374ZM12.53 9.22a.75.75 0 1 0-1.06 1.06L13.19 12l-1.72 1.72a.75.75 0 1 0 1.06 1.06l1.72-1.72 1.72 1.72a.75.75 0 1 0 1.06-1.06L15.31 12l1.72-1.72a.75.75 0 1 0-1.06-1.06l-1.72 1.72-1.72-1.72Z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </ButtonBox>
              {:else if button == "En"}
                <ButtonBox isPrimary={true} onClick={onEnterClick}>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                    class="size-6"
                    height="4rem"
                    width="4rem"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M16.5 3.75a1.5 1.5 0 0 1 1.5 1.5v13.5a1.5 1.5 0 0 1-1.5 1.5h-6a1.5 1.5 0 0 1-1.5-1.5V15a.75.75 0 0 0-1.5 0v3.75a3 3 0 0 0 3 3h6a3 3 0 0 0 3-3V5.25a3 3 0 0 0-3-3h-6a3 3 0 0 0-3 3V9A.75.75 0 1 0 9 9V5.25a1.5 1.5 0 0 1 1.5-1.5h6Zm-5.03 4.72a.75.75 0 0 0 0 1.06l1.72 1.72H2.25a.75.75 0 0 0 0 1.5h10.94l-1.72 1.72a.75.75 0 1 0 1.06 1.06l3-3a.75.75 0 0 0 0-1.06l-3-3a.75.75 0 0 0-1.06 0Z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </ButtonBox>
              {/if}
            {/each}
          </div>
        {/each}
      </div>
    </div>
    <div class="mgn-top">
      <span
        >Your Record: <span style:color={theme.palette.primary}>{record}</span
        ></span
      >
    </div>
  </div>
{:else}
  <div></div>
{/if}

<style>
  .content {
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  .mgn-top {
    margin-top: 0.25rem;
    font-size: 1.4rem;
  }

  .number {
    font-weight: 400;
  }

  .render {
    font-size: 2.7rem;
  }

  @media (max-width: 392px) {
    .render {
      font-size: 2rem;
    }
  }
</style>
