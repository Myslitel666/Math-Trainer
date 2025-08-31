<script lang="ts">
  import { themeStore, themeMode } from "svelte-elegant/stores";
  import { onMount } from "svelte";
  import ButtonBox from "svelte-elegant/ButtonBox";

  let exampleColor = "";
  let firstNumber = 0;
  let secondNumber = 0;
  let memoryItems = "Numbers and Letters"; // значение по умолчанию
  let isInitialized = false;
  let timeLeft = 60; // 60 секунд = 1 минута
  let timerInterval: number | null = null;

  let inputStr = "";
  let isError = 0;
  let record = 0;
  let rightColor = "";
  let errColor = "";
  let num = "";
  let textRender = "";

  let operation = "";
  let operations = "+-•÷";
  let operationsHist = "";

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

  timerInterval = setInterval(() => {
    if (timeLeft > 0) {
      timeLeft--;
    }
  }, 1000);

  function checkResult() {
    if (inputStr === num) {
      isError = 0;
    } else {
      isError = 1;
    }
    inputStr = "";

    genExample();
  }

  function genOperation() {
    let isRepeatOperation = true;
    let operationInd = 0;

    while (isRepeatOperation) {
      operationInd = Math.floor(Math.random() * operations.length);

      if (operationsHist.length > 2) {
        if (
          operations[operationInd] !==
            operationsHist[operationsHist.length - 1] ||
          operations[operationInd] !== operationsHist[operationsHist.length - 2]
        ) {
          isRepeatOperation = false;
        }
      } else {
        isRepeatOperation = false;
      }
    }
    operationsHist += operations[operationInd];

    return operations[operationInd];
  }

  function genComplexNumber(min: number, max: number) {
    let isNotComplex = true;
    let num = 0;

    while (isNotComplex) {
      num = Math.floor(Math.random() * (max - min + 1)) + min;

      if (num % 10 !== 0) {
        isNotComplex = false;
      }
    }

    return num;
  }

  function genSubOperand(firstOperand: number, min: number, max: number) {
    let isNotComplex = true;
    let num = 0;
    let firstOperandAbs = Math.abs(firstOperand);

    while (isNotComplex) {
      num = genComplexNumber(min, max);
      let secondOperandAbs = Math.abs(num);
      const delta = Math.abs(secondOperandAbs - firstOperandAbs);

      if (delta > 10) {
        isNotComplex = false;
      }
    }

    return num;
  }

  function genExample() {
    operation = genOperation();

    //operation = "-";

    if (operation === "+") {
      firstNumber = genComplexNumber(11, 99);
      secondNumber = genComplexNumber(11, 99);

      num = (firstNumber + secondNumber).toString();
    } else if (operation === "-") {
      firstNumber = genComplexNumber(11, 99);
      secondNumber = genSubOperand(firstNumber, 11, 99);

      num = (firstNumber - secondNumber).toString();
    } else if (operation === "•") {
      secondNumber = genComplexNumber(3, 9);

      if (secondNumber === 3) {
        firstNumber = genComplexNumber(34, 99);
      } else {
        firstNumber = genComplexNumber(11, 99);
      }
      num = (firstNumber * secondNumber).toString();
    } else {
      let quotient = genComplexNumber(11, 99);
      let divisor = genComplexNumber(3, 9);
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
    num = num.replace(/-/g, "");

    if (inputStr.length > num.length) onBackClick();

    inputStr = inputStr.toLocaleUpperCase();

    textRender = inputStr;
    // Добавляем маскирующие символы
    const dotsToAdd = Math.max(0, num.length - textRender.length);
    textRender += "•".repeat(dotsToAdd);

    exampleColor = isError ? errColor : rightColor;
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
    <div class="counts">
      <span style:margin-top="-5px" style:color={rightColor}>✔0</span>
      <span style:color={timeLeft > 10 ? rightColor : errColor}>
        {Math.floor(timeLeft / 60)
          .toString()
          .padStart(2, "0")}:{(timeLeft % 60).toString().padStart(2, "0")}
      </span>
      <span style:margin-top="-4.5px" style:color={errColor}>✘0</span>
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

  .counts {
    display: flex;
    justify-content: space-between;
    font-size: 40px;
    width: 18.5rem;
  }

  @media (max-width: 392px) {
    .render {
      font-size: 2rem;
    }
  }
</style>
