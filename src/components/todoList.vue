<template>
  <div>
    <div class="todo__body" v-for="(todo, i) in search" :key="i">
      <p class="todo__number">{{ i + 1 }}</p>
      <p class="todo__text" :class="{ todo__text_isDone: todo.isComplite }">
        {{ todo.text }}
      </p>
      <div class="todo__btn-wrap">
        <!-- <input class="todo__btn green" v-model="inProcess" type="checkbox" /> -->
        <input
          class="todo__btn blue"
          v-model="todo.isComplite"
          type="checkbox"
        />
        <button @click="removeItem(i)" class="todo__btn red">X</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {

  props: {
    todos: {
      type: [Object, Array],
      required: true,
    },
    search:{
          type: [Object, Array,String],
    },
    removeTodo: {
      type: Function,
    },
  },
  methods: {
    removeItem(i) {
      this.$emit("removeTodo", i);
    },

  },
};
</script>