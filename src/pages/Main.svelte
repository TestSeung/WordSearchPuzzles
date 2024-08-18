<script>
  let rows = 12;
  let columns = 12;

  // 사용자가 찾을 단어 목록
  const words = [
    "HELLO",
    "WORLD",
    "SVELTE",
    "PUZZLE",
    "SEARCH",
    "DRAG",
    "MOUSE",
    "GRID",
    "WORDS",
    "FIND",
    "ALPHA",
    "CODE",
  ];

  // 알파벳 배열 정의
  const alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

  // 2D 배열을 생성하여 셀을 알파벳으로 초기화
  let grid = Array.from({ length: rows }, () =>
    Array.from(
      { length: columns },
      () => alphabet[Math.floor(Math.random() * alphabet.length)]
    )
  );

  // 맞춘 단어의 셀을 추적
  let correctCells = new Set();

  // 선택된 셀의 위치를 저장하는 배열
  let selectedCells = [];

  let isDragging = false;

  // 마우스 드래그 시작
  let startPos = null;

  // 단어를 퍼즐에 배치하는 함수
  function placeWordsInGrid(words) {
    let row = 0;
    for (let word of words) {
      // 단어를 격자의 한 줄에 배치합니다. // 단어를 가로 방향으로 배치
      for (let col = 0; col < word.length; col++) {
        grid[row][col] = word[col];
      }
      row++; // 다음 줄로 이동
    }
  }

  // 단어를 퍼즐에 배치
  placeWordsInGrid(words);

  // 마우스 드래그 이벤트 처리
  function handleMouseDown(row, col) {
    startPos = { row, col };
    isDragging = true;
    selectedCells = [{ row, col }]; // 드래그 시작 시 시작 셀 추가
  }

  function handleMouseUp(row, col) {
    if (isDragging) {
      isDragging = false;
      checkWordSelection();
      startPos = null;
    }
  }
  function handleMouseOver(row, col) {
    if (isDragging) {
      selectedCells = calculateSelection(startPos, { row, col });
    }
  }
  // 단어 선택을 확인하는 함수
  function calculateSelection(start, end) {
    const cells = [];
    if (start.row === end.row) {
      for (
        let i = Math.min(start.col, end.col);
        i <= Math.max(start.col, end.col);
        i++
      ) {
        cells.push({ row: start.row, col: i });
      }
    } else if (start.col === end.col) {
      for (
        let i = Math.min(start.row, end.row);
        i <= Math.max(start.row, end.row);
        i++
      ) {
        cells.push({ row: i, col: start.col });
      }
    }
    return cells;
  }

  function checkWordSelection() {
    if (selectedCells.length === 0) return;

    let selectedWord = "";
    const { row: startRow, col: startCol } = selectedCells[0];
    const { row: endRow, col: endCol } =
      selectedCells[selectedCells.length - 1];

    if (startRow === endRow) {
      // 가로로 선택된 경우
      for (
        let i = Math.min(startCol, endCol);
        i <= Math.max(startCol, endCol);
        i++
      ) {
        selectedWord += grid[startRow][i];
      }
    } else if (startCol === endCol) {
      // 세로로 선택된 경우
      for (
        let i = Math.min(startRow, endRow);
        i <= Math.max(startRow, endRow);
        i++
      ) {
        selectedWord += grid[i][startCol];
      }
    }

    if (words.includes(selectedWord)) {
      alert(`Word found: ${selectedWord}`);
      selectedCells.forEach((cell) =>
        correctCells.add(`${cell.row},${cell.col}`)
      );
    } else {
      alert("Word not found!");
    }

    selectedCells = []; // 선택 초기화
  }
  function isCorrectCell(row, col) {
    return correctCells.has(`${row},${col}`);
  }
</script>

<div class="container">
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div
    class="grid-container"
    style="--rows: {rows}; --columns: {columns}"
    on:mouseup={handleMouseUp}
  >
    {#each grid as row, rowIndex}
      {#each row as cell, columnIndex}
        <!-- svelte-ignore a11y-mouse-events-have-key-events -->
        <button
          class="grid-item {selectedCells.some(
            (cell) => cell.row === rowIndex && cell.col === columnIndex
          )
            ? 'selected'
            : ''}{isCorrectCell(rowIndex, columnIndex) ? 'correct' : ''}"
          on:mousedown={() => handleMouseDown(rowIndex, columnIndex)}
          on:mouseover={() => handleMouseOver(rowIndex, columnIndex)}
        >
          {cell}
        </button>
      {/each}
    {/each}
  </div>
  <div class="checklist">
    현황판
    <ul>
      {#each words as word}
        <li class={correctCells.has(word) ? "found" : "not-found"}>{word}</li>
      {/each}
    </ul>
  </div>
</div>

<style>
  .container {
    display: grid;
    grid-template-columns: 1fr 3fr 1fr; /* 좌측, 중앙, 우측 레이아웃 */
    gap: 10px;
    padding: 10px;
    margin-top: 100px;
  }
  .grid-container {
    display: grid;
    grid-template-columns: repeat(var(--columns), 50px);
    grid-template-rows: repeat(var(--rows), 50px);
    gap: 1px;
    width: 100%;
    height: 100%;
    max-width: 500px;
    max-height: 500px;
    text-align: center;
  }

  .grid-item {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid #ccc;
    background-color: #f1f1f1;
    font-size: 24px;
    font-weight: bold;
    color: #333;
    cursor: pointer;
    height: 100%;
    user-select: none; /* 드래그 시 텍스트가 선택되지 않도록 설정 */
  }

  .grid-item.selected {
    background-color: yellow; /* 선택된 셀의 배경색 변경 */
  }

  .grid-item.correct {
    background-color: lightgreen; /* 맞춘 단어의 색상 */
  }

  .checklist {
    text-align: right;
  }

  .checklist .found {
    color: green;
    text-decoration: line-through;
  }
</style>
