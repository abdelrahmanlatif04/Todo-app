<template>
  <div class="body w-full h-[100vh] relative">
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
        v-text="i + 1 + '. ' + task"
        @click="tasks.splice(i, 1)"
        class="cursor-pointer hover:opacity-70 p-2 bg-gray-200 rounded-lg"
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
  },
};
</script>

<style scoped>
.width {
  @apply min-w-96 max-w-[600px] w-3/5 left-1/2 -translate-x-1/2;
}
</style>
