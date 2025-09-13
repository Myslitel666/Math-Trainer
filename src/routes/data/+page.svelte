<script>
  import { Box, Button, AutoComplete } from "svelte-elegant";
  import { Process, Info, DiagramIconPro } from "svelte-elegant/icons-elegant";

  let metric = "Max Power";
</script>

<div class="page">
  <div class="box">
    <p class="header">Analytical Data</p>
    <p>
      Your progress is saved automatically in your browser. Only you can see it.
    </p>
    <div class="box-list">
      {#if metric === "Max Power"}
        <Box variant="Hoverable" maxWidth="500px" width="100vw">
          <div class="box">
            <p style:font-size="20px">Max Power</p>
            <p class="score">20.35</p>
            <div class="info-icon">
              <Info />
            </div>
          </div>
        </Box>
      {:else}
        <Box variant="Hoverable" maxWidth="500px" width="100vw">
          <div class="box">
            <p style:font-size="20px">Average</p>
            <p class="score">12.45</p>
            <div class="info-icon">
              <div class="info-icon-container">
                <Info />
              </div>
            </div>
          </div>
        </Box>
      {/if}
    </div>
    <p class="header" style:margin-top="30px">Configure Analytics</p>
    <div
      style:display="flex"
      style:align-items="center"
      style:margin-top="5px"
      style:margin-left=""
      style:gap="5px"
    >
      <p>Metric:</p>
      <AutoComplete
        label="Metric"
        width="450px"
        bind:value={metric}
        options={["Max Power", "Average"]}
      />
    </div>
    <div
      style:display="flex"
      style:gap="10px"
      style:margin-top="7px"
      style:margin-left="9px"
    >
      <div style:display="flex" style:align-items="center" style:gap="5px">
        <p style:margin-left="-9px" style:margin-right="9px">Time:</p>
        <AutoComplete
          isSelect
          label="Time"
          width="200px"
          value="2 min"
          options={["1 min", "2 min"]}
        />
      </div>
      <div style:display="flex" style:align-items="center" style:gap="5px">
        <p>Level:</p>
        <AutoComplete
          label="Level"
          width="198px"
          value="Medium"
          options={["1", "2"]}
        />
      </div>
    </div>
    <Button
      variant="Text"
      padding="7px"
      height="40px"
      marginTop="5px"
      bgColorHover="rgb(255,0,0,0.12)"
      color="#ec1313"
      borderColor="#ec1313"
      maxWidth="500px"
      width="100vw"
    >
      <div style:margin-top="5px" style:margin-left="-2px">
        <Process fill="#ec1313" size="35px" />
      </div>
      Reset Current Data
    </Button>
    <DiagramIconPro strokeWidth="0.03" size="200px" />
  </div>
</div>

<style>
  .info-icon {
    position: absolute;
    right: 15px;
  }
  .page {
    padding-left: 15px;
    padding-right: 15px;
    padding-top: 10px;
  }

  .header {
    font-size: 24px;
    font-weight: 600;
  }

  .score {
    font-size: 45px;
    font-weight: 600;
  }

  .box-list {
    display: flex;
    flex-direction: column;
    margin-top: 3px;
  }

  .box {
    display: flex;
    position: relative;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 6px;
    padding-top: 5px;
    width: 100%;
  }
</style>
