<template>
  <div id="app">
    <h1>Todo list</h1>
    <input type="text" v-model="inputValue" v-on:keyup.enter="addInToList" />
    <button v-on:click="addInToList">Add</button>
    <div
      v-for="(list, index) in listsfiltered"
      :key="index"
      :class="{ completed: list.completed }"
    >
      <ul>
        <li>
          <input type="checkbox" v-model="list.completed" />
          {{ list.listDesc }}
          <button @click="removeList(index)" v-on:click="filter = 'all'">
            x
          </button>
        </li>
      </ul>
    </div>
    <div>
      <button class="btn" @click="clearComplete">ClearComplete</button>
      <button class="btn" @click="allcheck" v-on:click="filter = 'all'">
        Select All
      </button>
      <button class="btn" @click="filter = 'all'">All</button>
      <button class="btn" @click="filter = 'active'">Active</button>
      <button class="btn" @click="filter = 'complete'">Complete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      currentActive: true,
      inputValue: "",
      count: 1,
      filter: "all",
      lists: [],
    };
  },
  computed: {
    listsfiltered() {
      if (this.filter === "all") {
        return this.lists;
      } else if (this.filter === "active") {
        return this.lists.filter((list) => !list.completed);
      } else if (this.filter === "complete") {
        return this.lists.filter((list) => list.completed);
      }
    },
  },
  methods: {
    addInToList() {
      if (this.inputValue !== "") {
        const detail = {
          listId: this.count,
          listDesc: this.inputValue,
          completed: false,
        };
        this.lists.push(detail);
        this.inputValue = "";
        this.count++;
      }
    },
    removeList(index) {
      this.lists.splice(index, 1);
    },
    allcheck() {
      this.lists.forEach((list) => {
        if (list.completed) {
          list.completed = false;
        } else {
          list.completed = true;
        }
      });
    },
    clearComplete() {
      let newlists = [];
      this.lists.forEach((list, index) => {
        if (!list.completed) {
          newlists.push(list);
        }
        this.lists = newlists;
      });
    },
  },
};
</script>

<style>
ul {
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.26);
  border-radius: 10px;
  list-style: none;
  margin: 2rem;
}

h1 {
  font-size: 50px;
}

#app {
  text-align: center;
}

.btn {
  margin: 0px 10px;
  border-radius: 5px;
}
.btn:hover {
  background-color: green;
  transition: 4s;
}
.completed {
  text-decoration: line-through;
}

.empty {
  display: none;
}
</style>