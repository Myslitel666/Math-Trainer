<script>
  import { Box, Button } from "svelte-elegant";
</script>

<div class="page">
  <div class="box">
    <p class="header">Analytical Data</p>
    <p>
      Your progress is saved automatically in your browser. Only you can see it.
    </p>
    <div class="box-list">
      <Box variant="Hoverable" maxWidth="500px" width="100vw">
        <div class="box">
          <p style:font-size="20px">Max Power</p>
          <p class="score">20.35</p>
        </div>
      </Box>
      <Box variant="Hoverable" maxWidth="500px" width="100vw">
        <div class="box">
          <p style:font-size="20px">Average</p>
          <p class="score">12.45</p>
        </div>
      </Box>
      <Button
        variant="Outlined"
        color="#ec1313"
        borderColor="#ec1313"
        maxWidth="500px"
        width="100vw">Delete Data</Button
      >
    </div>
  </div>
</div>

<style>
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
    margin-top: 7px;
    gap: 15px;
  }

  .box {
    display: flex;
    flex-direction: column;
    justify-self: center;
    align-items: center;
    gap: 6px;
    padding-top: 5px;
  }
</style>
