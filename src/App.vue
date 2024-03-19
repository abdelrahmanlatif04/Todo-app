<template>
  <div class="body w-full h-[100vh] relative md:px-5">
    <h1
      class="text-6xl text-center tracking-widest font-semibold text-gray-500 pt-10"
    >
      TODO APP
    </h1>
    <div
      class="field h-11 width bg-red-600 relative top-14 rounded-md overflow-hidden"
    >
      <button
        @click="addTask()"
        class="h-full text-sm absolute right-0 font-semibold2 text-gray-200 hover:tracking-widest aspect-square duration-300 transition-all bg-orange-400 bg-[../] z-10 hover:opacity-70"
      >
        Add task
      </button>
      <input
        type="text"
        class="w-full pl-4 py-2 pr-12 text-xl bg-gray-200 outline-none"
        @keydown="pressed()"
        v-model="val"
      />
    </div>
    <ul class="relative top-20 width flex flex-col gap-2">
      <li
        v-for="(task, i) in tasks"
        :key="task"
        v-text="task"
        @click="completeTask(i)"
        @contextmenu="deleteTask(i)"
      ></li>
      <li
        class="completed"
        v-for="(task, i) in comTasks"
        :key="task"
        v-text="task"
      ></li>
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
  methods: {
    addTask() {
      if (this.val) {
        // push _not unshift()_ to add the new task at the end of line
        this.tasks.push(this.val);
        this.val = null;
      }
    },
    pressed() {
      if (event.key === "Enter") {
        this.addTask();
      }
    },
    completeTask(i) {
      this.comTasks.push(this.tasks[i]);
      this.deleteTask(i);
    },
    deleteTask(i) {
      this.tasks.splice(i, 1);
    },
  },
};
</script>

<style scoped>
.width {
  @apply min-w-80 max-w-[600px] w-3/5 left-1/2 -translate-x-1/2;
}
li {
  @apply p-2 bg-gray-200 rounded-lg relative cursor-pointer hover:opacity-70;
}
li::before {
  content: "";
  @apply -translate-x-1/2 -translate-y-1/2 top-5 right-0 rounded-sm absolute aspect-square h-5 border-gray-600 border bg-gray-200;
}
li.completed::before {
  content: url("../src/assets/checkmark-outline.svg");
  @apply -translate-x-1/2 -translate-y-1/2 top-5 right-0 rounded-sm absolute aspect-square h-5 border-gray-600 border bg-gray-200 bg-gradient-to-tl from-orange-300 to-gray-300;
}
</style>
