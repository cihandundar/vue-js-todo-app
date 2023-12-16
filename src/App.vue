<template>
  <div>
    <Todo />
  </div>
</template>

<script>
import Todo from "./components/Todo.vue";
export default {
  name: "App",
  components: { Todo },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Yusei+Magic&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  list-style-type: none;
  font-family: "Inter", sans-serif;
}
body {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  background-color: #2c3e50;
  color: #fff;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
