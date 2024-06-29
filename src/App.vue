<template>
  <div class="body w-full h-[100vh] relative md:px-5">
    <h1
      class="text-6xl text-center relative tracking-widest font-semibold text-gray-500 pt-10"
    >
      TODO APP
    </h1>
    <div
      class="field h-11 width bg-red-600 relative top-14 rounded-md overflow-hidden"
    >
      <button
        @click="addTask()"
        class="h-full text-sm absolute right-0 font-semibold2 text-gray-200 hover:tracking-widest px-2 aspect-square duration-300 transition-all bg-orange-400 z-10 hover:opacity-70"
      >
        Add task
      </button>
      <input
        :placeholder="`Ex : ${placeHolding()}`"
        type="text"
        class="w-full pl-4 py-2 pr-12 text-xl bg-gray-200 outline-none"
        @keydown="pressed()"
        v-model="val"
      />
    </div>
    <ul class="relative top-20 width flex flex-col gap-2">
      <li v-for="(task, i) in tasks" :key="task">
        <div class="overflow-hidden" @click="completeTask(i)">{{ task }}</div>
        <img
          @click="deleteTask(this.tasks, i)"
          src="../src/assets/trash-outline.svg"
        />
      </li>
      <li
        class="completed text-gray-600 line-through"
        v-for="(task, i) in comTasks"
        :key="task"
      >
        <div class="overflow-hidden">{{ task }}</div>
        <img
          @click="deleteTask(this.comTasks, i)"
          src="../src/assets/trash-outline.svg"
        />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      val: "",
      tasks: [],
      comTasks: [],
      invalidValue: false,
    };
  },
  created() {
    this.bringDataBack();
  },
  methods: {
    addTask() {
      if (this.val) {
        // push _not unshift()_ to add the new task at the end of line
        this.tasks.push(this.val);
        this.val = null;
        this.storeTasks();
      }
    },
    pressed() {
      if (event.key === "Enter") {
        this.addTask();
      }
    },
    completeTask(i) {
      if (this.tasks[i]) {
        this.comTasks.push(this.tasks[i]);
        this.deleteTask(this.tasks, i);
        this.storeTasks();
      }
    },
    deleteTask(arr, i) {
      arr.splice(i, 1);
      this.storeTasks();
    },
    storeTasks() {
      localStorage.setItem("incompleted tasks", JSON.stringify(this.tasks));
      localStorage.setItem("completed tasks", JSON.stringify(this.comTasks));
    },
    bringDataBack() {
      let completedTasks = localStorage.getItem("completed tasks");
      let inCompletedTasks = localStorage.getItem("incompleted tasks");
      if (completedTasks.length > 2) {
        this.comTasks = JSON.parse(completedTasks);
      }
      if (inCompletedTasks.length > 2) {
        this.tasks = JSON.parse(inCompletedTasks);
      }
    },
    placeHolding() {
      let placeholdersExamples = [
        "Convince the cat to do the dishes",
        "Practice my Oscar acceptance speech",
        "Teach the toaster to make coffee",
        "Perfect my ninja skills",
        "Stare at a wall for world record attempt.",
        "Find sock thief in laundry.",
        "Broker coffee-alarm peace deal.",
      ];
      return placeholdersExamples[Math.floor(Math.random() * 6)];
  },
      },
};
</script>

<style scoped>
h1::before {
  content: "Procrastinate like a pro";
  @apply absolute -bottom-10 tracking-wide text-orange-500 left-1/2 text-lg -translate-x-1/2 w-full;
}
.width {
  @apply min-w-80 max-w-[600px] w-3/5 left-1/2 -translate-x-1/2;
}
li {
  @apply p-2 bg-gray-200 rounded-lg relative cursor-pointer;
}
li > div::before {
  content: "";
  @apply -translate-x-1/2 -translate-y-1/2 top-5 right-0 rounded-sm absolute aspect-square h-5 border-gray-600 border bg-gray-200;
}
li.completed > div::before {
  content: url("../src/assets/checkmark-outline.svg");
  @apply -translate-x-1/2 -translate-y-1/2 top-5 right-0 rounded-sm absolute aspect-square h-5 border-gray-600 border bg-gray-200 bg-gradient-to-tl from-orange-300 to-gray-300;
}
img {
  @apply h-1/2 absolute top-1/2 -right-5 sm:-right-8 -translate-y-1/2 hover:scale-125 transition duration-300;
}
</style>
