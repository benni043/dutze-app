<script setup lang="ts">
import {ref} from "vue";

type Cell = {
  id: number;
  x: number;
  y: number;
  color: string | null;
};

const rows = ref(5);
const cols = ref(6);
const selectedColor = ref("#3498db");
const grid = ref<Cell[]>([]);
const note = ref("");
const isDark = ref(false);

const initGrid = () => {
  const newGrid: Cell[] = [];
  let id = 0;

  for (let r = 0; r < rows.value; r++) {
    for (let c = 0; c < cols.value; c++) {
      const existing = grid.value.find((cell) => cell.x === r && cell.y === c);
      if (existing) {
        newGrid.push(existing);
      } else {
        newGrid.push({id: id++, x: r, y: c, color: null});
      }
    }
  }
  grid.value = newGrid;
};

const toggleCell = (cell: Cell) => {
  cell.color = cell.color ? null : selectedColor.value;
};

const resetGrid = () => {
  grid.value.forEach((cell) => (cell.color = null));
};

const toggleDark = () => {
  isDark.value = !isDark.value;
  document.body.classList.toggle("dark", isDark.value);
};


initGrid();
</script>

<template>
  <div class="container" :class="{ dark: isDark }">
    <!-- Navbar -->
    <nav class="navbar">
      <div class="nav-controls">
        <div class="nav-item">
          <label>Reihen</label>
          <input
              type="number"
              v-model.number="rows"
              min="1"
              @change="initGrid"
          />
        </div>

        <div class="nav-item">
          <label>Spalten</label>
          <input
              type="number"
              v-model.number="cols"
              min="1"
              @change="initGrid"
          />
        </div>

        <div class="nav-item">
          <label>Farbe</label>
          <input type="color" v-model="selectedColor"/>
        </div>
      </div>

      <div class="nav-actions">
        <div class="nav-item">
          <button @click="resetGrid" class="reset-btn">Reset</button>
        </div>

        <div class="nav-item">
          <button @click="toggleDark" class="dark-toggle">
            {{ isDark ? "☀️ Light" : "🌙 Dark" }}
          </button>
        </div>

      </div>
    </nav>

    <!-- Grid -->
    <div
        class="grid"
        :style="{
        gridTemplateRows: `repeat(${rows}, 1fr)`,
        gridTemplateColumns: `repeat(${cols}, 1fr)`
      }"
    >
      <div
          v-for="cell in grid"
          :key="cell.x + '-' + cell.y"
          class="cell"
          @click="toggleCell(cell)"
          :style="{ backgroundColor: cell.color || 'transparent' }"
      ></div>
    </div>

    <!-- notes -->
    <div class="notes">
      <label>Notizen</label>
      <textarea
          v-model="note"
          rows="5"
          placeholder="Schreib hier deine Notizen..."
      ></textarea>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
  background-color: white;
  color: black;
}

body.dark {
  background-color: #121212;
  color: #e0e0e0;
}

.container {
  display: flex;
  flex-direction: column;
  height: 90vh;
  gap: 50px;
  margin: 5vh 5vw;
}

.navbar {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.nav-controls,
.nav-actions {
  display: flex;
  flex: 1;
  gap: 10px;
}

.nav-item {
  flex: 1;
  min-width: 80px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
}

.nav-item input,
.nav-item button {
  width: 100%;
  min-height: 30px;
  box-sizing: border-box;
  padding: 6px;
  border-radius: 60px;
  border: 1px solid #aaa;
  text-align: center;
  font-size: 18px;
}

body.dark .nav-item input,
body.dark .nav-item button {
  background-color: #1e1e1e;
  color: #f0f0f0;
  border: 1px solid #555;
}

.notes {
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex-grow: 1;
  min-height: 0;
  margin-bottom: 10px;
}

.notes textarea {
  flex-grow: 1;
  resize: none;
  padding: 10px;
  border-radius: 6px;
  border: 1px solid #aaa;
  font-family: inherit;
  font-size: 16px;
}

body.dark .notes textarea {
  background-color: #1e1e1e;
  color: #f0f0f0;
  border: 1px solid #555;
}

.reset-btn {
  background-color: red;
}

body.dark .reset-btn {
  background-color: red!important;
}

/* GRID */
.grid {
  overflow: auto;
  display: grid;
  gap: 6px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.cell {
  border: 1px solid #999;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
  aspect-ratio: 1 / 1;
}

</style>
