<template>
  <div class="wrapper">
    <h1>Todo List - warliic35</h1>
    <div ref="complatedRef" class="complatedDiv">
      <p style="color: white">
        Total Task: {{ data.length }} - Complated Task: {{ getComplated }}
      </p>
    </div>
    <div class="inputContainer">
      <input
        type="text"
        ref="myInput"
        class="todoInput"
        v-model="model"
        placeholder="Enter new todo"
      />
      <button @click="addTodo" class="addBtn">Add Todo</button>
    </div>
    <ul>
      <li v-for="(item, index) in data" :key="index">
        <span :class="{ strike: item.complated }">{{ item.text }}</span>
        <div class="btnGroup">
          <button @click="doneTodo(item)" class="doneBtn">Done</button>
          <button @click="deleteTodo(index)" class="deleteBtn">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Todo",

  data() {
    return {
      model: "",
      data: [],
    };
  },

  computed: {
    getComplated() {
      return this.data.filter((item) => item.complated).length;
    },
  },

  watch: {
    getComplated(newVal) {
      if (newVal / this.data.length >= 0.5) {
        this.$refs.complatedRef.style.background = "green";
      } else {
        this.$refs.complatedRef.style.background = "red";
      }
    },
  },

  mounted() {
    this.getTodosFromLocalStorage();
  },

  methods: {
    getTodosFromLocalStorage() {
      const savedTodos = JSON.parse(localStorage.getItem("todos"));
      if (savedTodos) {
        this.data = savedTodos;
      }
    },

    saveTodosToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.data));
    },

    addTodo() {
      if (this.model !== "") {
        this.data.push({
          id: Date.now(),
          text: this.model,
          complated: false,
        });
        this.saveTodosToLocalStorage();
      }
      this.model = "";
      this.$refs.myInput.focus();
    },

    deleteTodo(index) {
      this.data.splice(index, 1);
      this.saveTodosToLocalStorage();
    },

    doneTodo(item) {
      const idx = this.data.findIndex((x) => x.id === item.id);
      this.data[idx].complated = !item.complated;
      this.saveTodosToLocalStorage();
    },
  },
};
</script>

<style>
.strike {
  text-decoration: line-through;
  color: green;
}
.wrapper {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.inputContiner {
  display: flex;
  justify-content: center;
  align-items: center;
}

.todoInput {
  width: 70%;
  padding: 10px;
  border: none;
  outline: none;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}

ul {
  margin-top: 10px;
  background-color: #fff;
  width: 100%;
  border-radius: 10px;
}

ul li {
  color: #000;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
}

.addBtn {
  border: none;
  padding: 10px;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
  background-color: green;
  color: #fff;
  cursor: pointer;
}

.btnGroup {
  display: flex;
  justify-content: center;
  gap: 2px;
}

.deleteBtn {
  background-color: #fff;
  color: red;
  border: none;
  cursor: pointer;
  padding: 8px;
  transition: 0.3s ease-in;
}

.deleteBtn:hover {
  background-color: red;
  color: #fff;
}

.doneBtn {
  background-color: #fff;
  color: green;
  border: none;
  cursor: pointer;
  padding: 8px;
  transition: 0.3s ease-in;
}

.doneBtn:hover {
  background-color: green;
  color: #fff;
}

.complatedDiv {
  padding: 1rem;
  margin-block: 30px;
  border-radius: 10px;
}
</style>
