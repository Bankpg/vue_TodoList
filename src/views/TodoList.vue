<template>
  <div class="main">

    <div class="card addTodo">
      <h1>TODOLIST</h1>
      <span style="height: 30px;"></span>
      <!-- Input form for new tasks -->
      <form @submit.prevent="addTodo" style="display: flex;flex-direction: column; align-items: center;">
        <input type="text" class="form-control" id="text" placeholder="Enter todo here." v-model="todoText">
        <p>Dead line</p>
        <input type="date" id="deadline" placeholder="Enter task here." v-model="deadline">
        <span style="height: 30px;"></span>
        <button type="submit">Add Todo</button>
      </form>
    </div>

    <div class="card list" v-show="showDiv">
      <div
        style="background: linear-gradient(to right, #4395ff, #003dd8);width: 100%;height: 50px;border-radius: 20px 20px 0px 0px; display: flex; justify-content: space-between; align-items: center;">
        <h2 style="margin: 0px 20px;"><font-awesome-icon :icon="['fas', 'list-ol']" /></h2>
        <font-awesome-icon @click="swapDiv()" :icon="['fas', 'toggle-off']" class="toggle" />
      </div>

      <div style="overflow: scroll; height: 500px;">
        <li v-for="(todo, index) in todos" :key="index" style="color: white;">
          <div class="item todo" @click="completeTodo(index)" style="cursor: pointer;">
            <div
              style="background: #4395ff;display:flex;align-self:stretch;align-items: center;border-radius:20px 0px 0px 20px;">
              <font-awesome-icon :icon="['fas', 'book']" style="font-size: 30;margin: 20px;" />
            </div>
            <span
              style="display: flex;flex-direction: column;gap: 10px;width: 60%;word-wrap: break-word;margin: 10px 0px;">
              <p style="font-size: 15px; margin-top: 10px;"> {{ todo.text }} </p>
              <p style="font-size: 12px;color: #005e94;">Deadline {{ todo.deadline }} ( {{ countdown(todo.deadline) }} )
              </p>
            </span>
            <i class="fa-solid fa-trash-can" @click="removeTodo(index)"
              style="cursor: pointer; color: rgb(255, 0, 0);margin: 20px;"></i>
          </div>
        </li>
      </div>
    </div>

    <div class="card completed" v-show="!showDiv"> <!-- TODO COMPLETEED -->
      <div
        style="background: linear-gradient(to right, #34ff48, #00ca03);width: 100%;height: 50px;border-radius: 20px 20px 0px 0px; display: flex; justify-content: space-between; align-items: center;">
        <h2 style="margin: 0px 20px;"><font-awesome-icon :icon="['fas', 'list-check']" /></h2>
        <font-awesome-icon @click="swapDiv()" :icon="['fas', 'toggle-on']" class="toggle" />
      </div>

      <div style="overflow: scroll; height: 500px;">
        <li v-for="(completedTodo, index) in completedTodos" :key="index" style="color: white;">
          <div class="item">
            <div
              style="background: #0fe200;display:flex;align-self:stretch;align-items: center;border-radius:20px 0px 0px 20px;">
              <font-awesome-icon :icon="['far', 'circle-check']" style="font-size: 30;margin: 20px;" />
            </div>
            <span
              style="display: flex;flex-direction: column;gap: 10px;width: 60%;word-wrap: break-word;margin: 10px 0px;">
              <p style="font-size: 15px; margin-top: 10px;"> {{ completedTodo.text }} </p>
              <p style="font-size: 12px;color: #005e94;">Done {{ completedTodo.completedAt }} </p>
            </span>
            <i class="fa-solid fa-trash-can" @click="removeCompleted(index)"
              style="cursor: pointer; color: rgb(255, 0, 0);margin: 20px;"></i>
          </div>
        </li>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      todoText: '',
      deadline: '',
      showDiv: true,
      todos: [
        { text: "ส่งสมุด", deadline: "2024-1-16" },
        { text: "คุยงานกลุ่มภาษาไทย", deadline: "2024-2-17" },
        { text: "ซ้อมดนตรี", deadline: "2024-2-25" }

      ],
      completedTodos: [
        { text: "การบ้าน ดนตรี", completedAt: "14/1/2567 20:00:31" }
      ]
    };
  },
  methods: {
    swapDiv() {
      this.showDiv = !this.showDiv;
    },
    addTodo() {
      if (this.todoText && this.deadline) {
        this.todos.push({ text: this.todoText, deadline: this.deadline });
        this.todos.sort((a, b) => new Date(a.deadline) - new Date(b.deadline));
        this.todoText = '';
        this.deadline = '';
      }
    },
    removeTodo(index) {

      this.todos.splice(index, 1);

    },
    completeTodo(index) {
      const completedTodo = this.todos.splice(index, 1)[0];
      completedTodo.completedAt = new Date().toLocaleString();
      this.completedTodos.push(completedTodo);
    },
    removeCompleted(index) {
      this.completedTodos.splice(index, 1);
    },
    countdown(deadline) {
      const now = new Date();
      const targetDate = new Date(deadline);
      const diff = targetDate - now;

      const days = Math.floor(diff / (24 * 60 * 60 * 1000));
      const hours = Math.floor((diff % (24 * 60 * 60 * 1000)) / (60 * 60 * 1000));
      const minutes = Math.floor((diff % (60 * 60 * 1000)) / (60 * 1000));

      return `${days}d ${hours}h ${minutes}m`;
    },
  },
};
</script>
<style>
p {
  color: black;
}


input {
  margin: 5px 0px;
  padding: 10px 20px;
  font-size: 14px;
  border-radius: 8px;
  width: 100%;
  background-color: #eee;
  border: none;
  outline: none;
}

button {
  color: white;
  background-color: #0073ff;
  font-size: 15px;
  padding: 10px 30px;
  border-radius: 20px;
  font-weight: 800;
  text-transform: uppercase;
  margin-top: 10px;
  cursor: pointer;
}

button:hover {
  filter: brightness(0.9);
  transform: translateY(1%);

}

i:hover {
  transform: scale(1.1);
}

.main {
  display: flex;
  flex-wrap: wrap;
  min-height: 500px;
  min-width: 200px;
  gap: 50px;
  justify-content: center;
  align-items: center;
}

.card {
  box-shadow: 0 0 1px black;
  display: flex;
  justify-content: center;
  flex-direction: column;
  border-radius: 20px;
}

.card.addTodo {
  padding: 5% 90px;
  background-color: white;
}

.card.list,
.card.completed {
  background: #FFF;
  width: 500px;
  height: 550px;
  min-width: 360px;
}

.toggle {
  color: white;
  font-size: 30px;
  margin: 0px 20px;
  cursor: pointer;
}

.item {
  display: flex;
  justify-content: space-between;
  min-width: 300px;
  align-items: center;
  border-radius: 20px;
  background: #FFF;
  box-shadow: 0px 0px 4px 0px rgba(0, 0, 0, 0.25);
  margin: 5px 10%;
  opacity: 1;
  animation: fadeIn 1s ease-in-out;
}

.item:hover {
  transform: scale(1.01);
}

.item.todo:hover {
  filter: hue-rotate(-100deg);
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}
</style>