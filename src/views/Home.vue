
<template>
  <div id="app">
    <Header />
    <router-view />

    <div class="todo">
      <h3 class="title">Добавить задачу</h3>
      <div class="add-todo-wrap">
        <input
          v-model="todo"
          type="text"
          class="todo__input"
          placeholder="type todo..."
        />
        <button class="todo__btn green" @click="addTodo(id++)">+</button>
      </div>
      <h3 class="title">Поиск по задачам</h3>
      <div class="search">
        <input type="search" v-model="search" placeholder="Искать..." />
        <!-- <button @click="searchTodo"></button> -->
        <div>{{ search }}</div>
      </div>
      <h3 class="title">Список задач</h3>
      <div class="todo__body" v-for="(todo, i) in searchTodo" :key="i">
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
          <button @click="removeTodo(i)" class="todo__btn red">X</button>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
export default {
  data() {
    return {
      title: "ToDo app",
      id: 0,
      todo: "",
      todos: [],
      search: "",
      isDone: false,
      inProcess: false,
    };
  },

  async mounted() {
    const data = await localStorage.getItem("todos");
    if (data) {
      this.todos = JSON.parse(data);
    }
  },
  methods: {
    addTodo() {
      if (this.todo != "") {
        this.todos.push({
          id: this.id,
          text: this.todo,
          isComplite: this.isDone,
        });
        localStorage.setItem("todos", JSON.stringify(this.todos));
      }
      this.todo = "";
    },
    removeTodo(i) {
      this.todos.splice(i, 1);
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  computed: {
    searchTodo() {
      return this.todos.filter((elem) => {
        return elem.text.toLowerCase().includes(this.search);
      });
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.title {
  margin-bottom: 20px;
}
.search {
  margin-bottom: 20px;

  & input {
    padding: 10px;
    width: 100%;
    height: 30px;
    border: 1px solid gray;
    border-radius: 5px;
  }
}

.add-todo-wrap {
  margin-bottom: 20px;
}

.todo {
  width: 300px;
  padding-top: 30px;
  margin: 0 auto;
  border-radius: 5px;

  &__input {
    width: 253px;
    height: 30px;
    padding: 10px;
    border: 1px solid gray;
    border-radius: 5px;
    margin-right: 5px;
  }

  &__body {
    display: flex;
    align-items: center;
    width: 100%;
    margin-bottom: 10px;
  }
  &__number {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30px;
    height: 30px;
    border: 1px solid blue;
    border-radius: 5px;
  }

  &__text {
    font-size: 16px;
    padding-left: 10px;

    &_isDone {
      color: grey;
      text-decoration: line-through;
    }
  }
  &__btn-wrap {
    display: flex;
    align-items: center;
    margin-left: auto;
  }
  &__btn {
    width: 30px;
    height: 30px;
    border: none;
    border-radius: 5px;
    margin-left: 10px;

    &.green {
      background-color: green;
    }
    &.red {
      background-color: red;
    }
    &.blue {
      background-color: blue;
    }
  }
}
</style>
