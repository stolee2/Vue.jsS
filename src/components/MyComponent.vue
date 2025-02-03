<script setup>
import { ref, computed, watch } from "vue";

// 🎯 Реактивни променливи
const email = ref("");
const password = ref("");
const isLoggedIn = ref(false);
const errorMessage = ref("");

const count = ref(0);
const isOn = ref(false);
const name = ref("");
const message = ref("");

// 🎯 Список со задачи
const tasks = ref([
  { id: 1, title: "napisi kod za Vue.js", completed: false },
  { id: 2, title: "Izvrsi testiranje", completed: true },
  { id: 3, title: "Napisi dokumentacija", completed: false },
  { id: 4, title: "Isprati e-mail", completed: true },
]);

// 🎯 Филтер за задачи
const filter = ref("all");

// 🎯 Израчунато поле за филтрирање на задачи
const filteredTask = computed(() => {
  if (filter.value === "completed") {
    return tasks.value.filter((task) => task.completed);
  } else if (filter.value === "pending") {
    return tasks.value.filter((task) => !task.completed);
  }
  return tasks.value;
});

// 🎯 Функција за промена на статусот на задача
function toggleTaskCompletion(taskId) {
  const task = tasks.value.find((task) => task.id === taskId);
  if (task) {
    task.completed = !task.completed;
  }
}

// 🎯 Функција за додавање нова задача
function addTask() {
  if (newTask.value.trim() !== "") {
    tasks.value.push({
      id: tasks.value.length + 1,
      title: newTask.value,
      completed: false,
    });
    newTask.value = ""; // Испразни по додавање
  }
}

// 🎯 Реактивно поле за нова задача
const newTask = ref("");

// 🎯 Проверка дали формата е валидна
const isFormValid = computed(() => {
  return email.value.includes("@") && password.value.length >= 6;
});

// 🎯 Функција за најава
function login() {
  if (isFormValid.value) {
    isLoggedIn.value = true;
    errorMessage.value = "";
  } else {
    errorMessage.value = "Greska Proverete gi podatocite";
  }
}

// 🎯 Следење на email за проверка на валидност
watch(email, (newVal) => {
  if (!newVal.includes("@")) {
    errorMessage.value = "vnesete validna email adresa";
  } else {
    errorMessage.value = "";
  }
});

// 🎯 Функција за внесување име
function submitName() {
  if (name.value.trim() !== "") {
    message.value = name.value;
  } else {
    message.value = "Ve molime vnesete ime.";
  }
}

// 🎯 Функција за промена на статус
function toggle() {
  isOn.value = !isOn.value;
}

// 🎯 Функции за зголемување и намалување
function increment() {
  count.value++;
}

function decrement() {
  if (count.value > 0) {
    count.value--;
  }
}
</script>

<template>
  <!-- Бројач -->
  <div>
    <h2>Бројач: {{ count }}</h2>
    <button @click="increment">Зголеми</button>
    <button @click="decrement">Намали</button>
  </div>

  <!-- Статус -->
  <div>
    <h2>Status: {{ isOn ? "vkluceno" : "Iskluceno" }}</h2>
    <button @click="toggle">{{ isOn ? "Iskluci" : "Ukluci" }}</button>
  </div>

  <!-- Внесување име -->
  <div>
    <h2>Vnesete ime</h2>
    <input
      v-model="name"
      type="text"
      placeholder="Вашето име..."
      style="padding: 5px; font-size: 16px; width: 200px"
    />
    <button @click="submitName">Prikazi poraka</button>
    <h3>{{ message }}</h3>
  </div>

  <!-- Најава форма -->
  <div v-if="!isLoggedIn">
    <h2>Najavete se</h2>
    <input v-model="email" type="email" placeholder="vnesete email" />
    <input v-model="password" type="password" placeholder="Vnesete lozinka" />
    <button @click="login" :disabled="!isFormValid">Najava</button>
    <p v-if="errorMessage" style="color: red">{{ errorMessage }}</p>
  </div>

  <!-- Доколку е најавен, прикажи порака за добредојде -->
  <div v-else>
    <h2>Dobredojdovte, {{ email }}!</h2>
    <button @click="isLoggedIn = false">Odjavi se</button>
  </div>

  <!-- Додавање нова задача -->
  <div>
    <input v-model="newTask" type="text" placeholder="dodadi nova zadaca" />
    <button @click="addTask">Dodadi zadaca</button>
  </div>

  <!-- Филтрирање задачи -->
  <div>
    <button @click="filter = 'all'">site zadaci</button>
    <button @click="filter = 'completed'">Zavrseni zadaci</button>
    <button @click="filter = 'pending'">Nezavrseni zadaci</button>

    <ul>
      <li v-for="task in filteredTask" :key="task.id">
        <span
          :style="{ textDecoration: task.completed ? 'line-through' : 'none' }"
        >
          {{ task.title }}
        </span>
        <button @click="toggleTaskCompletion(task.id)">
          {{
            task.completed ? "Означи како не извршена" : "Означи како извршена"
          }}
        </button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
input {
  display: block;
  margin: 5px 0;
  padding: 8px;
  width: 250px;
}
button {
  padding: 8px;
  margin-top: 10px;
  cursor: pointer;
}
</style>
