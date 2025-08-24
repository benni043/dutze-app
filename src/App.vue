<script setup lang="ts">
import { ref } from "vue";

type Cell = {
  id: number;
  x: number;
  y: number;
  color: string | null;
};

const rows = ref(5);
const cols = ref(5);
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
        newGrid.push({ id: id++, x: r, y: c, color: null });
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
          <input type="color" v-model="selectedColor" />
        </div>
      </div>

      <div class="nav-actions">
        <button @click="resetGrid" class="reset-btn">Reset</button>
        <button @click="toggleDark" class="dark-toggle">
          {{ isDark ? "☀️ Light" : "🌙 Dark" }}
        </button>
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

<style scoped>
body {
  margin: 1px;
  padding: 0;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  background: #f0f0f0;
  color: #222;
  min-height: 100vh;
  transition: background 0.3s, color 0.3s;
}

/* DARK MODE */
.container.dark {
  background: #1e1e1e;
  color: #eee;
}

.container.dark .navbar {
  background: #0f0f0f;
}

.container.dark .grid {
  background: #252525;
  border-color: #333;
}

.container.dark .cell {
  border-color: #555;
}

.container.dark .notes textarea {
  background: #333;
  border-color: #555;
  color: #eee;
}

/* NAVBAR */
.navbar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
  gap: 20px;
  background: #3a3a3a;
  padding: 12px 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  color: white;
}

.nav-controls {
  display: flex;

  gap: 20px;
  flex-wrap: wrap;
}

.nav-item {
  display: flex;
  flex-direction: column;
  font-size: 13px;
  gap: 5px;
  align-items: center;
}

.nav-item input {
  width: 60px;
  border-radius: 4px;
  border: 1px solid #aaa;
}

.nav-item input[type="number"] {
  padding: 5px;
}

.nav-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.reset-btn,
.dark-toggle {
  padding: 8px 14px;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s;
  width: 85px;
}

.reset-btn {
  background: #e74c3c;
}
.reset-btn:hover {
  background: #c0392b;
}

.dark-toggle {
  background: #3498db;
}
.dark-toggle:hover {
  background: #2980b9;
}

/* GRID */
.grid {
  display: grid;
  gap: 6px;
  border: 1px solid #ccc;
  padding: 10px;
  background: #f9f9f9;
  border-radius: 8px;
  margin-bottom: 20px;
  transition: background 0.3s, border 0.3s;
}

.cell {
  border: 1px solid #999;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
  aspect-ratio: 1 / 1;
}

.cell:hover {
  opacity: 0.8;
}

/* NOTES */
.notes {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.notes textarea {
  padding: 10px;
  border-radius: 6px;
  border: 1px solid #aaa;
  resize: vertical;
  font-family: inherit;
  font-size: 14px;
  background: white;
  color: inherit;
  transition: background 0.3s, color 0.3s, border 0.3s;
}
</style>
