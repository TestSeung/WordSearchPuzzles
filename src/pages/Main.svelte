<script>
  let rows = 12;
  let columns = 12;

  // 2D 배열을 생성하여 셀을 관리
  let grid = Array.from({ length: rows }, () =>
    Array.from({ length: columns }, () => "")
  );
  // 특정 셀에 접근하고 수정하는 함수
  function handleCellClick(rowIndex, colIndex, value) {
    grid[rowIndex][colIndex] = `Clicked (${rowIndex}, ${colIndex})`;
    //grid[rowIndex][colIndex] = value;
  }
</script>

<div class="grid-container" style="--rows: {rows}; --columns: {columns}">
  {#each grid as row, rowIndex}
    {#each row as cell, columnIndex}
      <button
        class="grid-item"
        on:click={() => handleCellClick(rowIndex, columnIndex)}
      >
        {cell}
      </button>
    {/each}
  {/each}
</div>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: repeat(var(--columns), 50px);
    grid-template-rows: repeat(var(--rows), 50px);
    gap: 1px;
    width: 100%;
    height: 100%;
    max-width: 600px; /* 필요에 따라 크기 조정 */
    max-height: 600px; /* 필요에 따라 크기 조정 */
  }

  .grid-item {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid #ccc;
    background-color: #f9f9f9;
    font-size: 14px;
    cursor: pointer;
  }
</style>
