<script setup lang="ts">
import { ref } from "vue";

const rows = ref(5);
const cols = ref(5);
const color = ref("#3498db");
const grid = ref<boolean[][]>([]);
const note = ref("");

// Grid initialisieren
const initGrid = () => {
  grid.value = Array.from({ length: rows.value }, () =>
      Array.from({ length: cols.value }, () => false)
  );
};

const toggleCell = (r: number, c: number) => {
  grid.value[r][c] = !grid.value[r][c];
};

const resetGrid = () => {
  initGrid();
};

initGrid();
</script>

<template>
  <div class="container">
    <!-- Einstellungen -->
    <div class="controls">
      <label>
        Reihen
        <input type="number" v-model.number="rows" min="1" @change="initGrid" />
      </label>
      <label>
        Spalten
        <input type="number" v-model.number="cols" min="1" @change="initGrid" />
      </label>
      <label>
        Farbe
        <input type="color" v-model="color" />
      </label>
      <button @click="resetGrid" class="reset-btn">Reset</button>
    </div>

    <!-- Grid -->
    <div
        class="grid"
        :style="{
        gridTemplateRows: `repeat(${rows}, 1fr)`,
        gridTemplateColumns: `repeat(${cols}, 1fr)`
      }"
    >
      <div
          v-for="(cell, index) in rows * cols"
          :key="index"
          @click="toggleCell(Math.floor(index / cols), index % cols)"
          class="cell"
          :style="{
          backgroundColor: grid[Math.floor(index / cols)][index % cols]
            ? color
            : 'white'
        }"
      ></div>
    </div>

    <!-- Notizen -->
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
.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.controls {
  display: flex;
  gap: 20px;
  align-items: flex-end;
  flex-wrap: wrap;
  margin-bottom: 20px;
}

.controls label {
  display: flex;
  flex-direction: column;
  font-size: 14px;
}

.controls input[type="number"] {
  width: 60px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.controls input[type="color"] {
  width: 50px;
  height: 35px;
  border: none;
  padding: 0;
  background: none;
}

.reset-btn {
  padding: 8px 16px;
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
.reset-btn:hover {
  background: #c0392b;
}

.grid {
  display: grid;
  gap: 6px;
  border: 1px solid #ddd;
  padding: 10px;
  background: #f9f9f9;
  border-radius: 8px;
  margin-bottom: 20px;
}

.cell {
  border: 1px solid #bbb;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
  aspect-ratio: 1 / 1; /* macht die Felder quadratisch */
}

.cell:hover {
  opacity: 0.8;
}

.notes {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.notes textarea {
  padding: 10px;
  border-radius: 6px;
  border: 1px solid #ccc;
  resize: vertical;
  font-family: inherit;
  font-size: 14px;
}
</style>
