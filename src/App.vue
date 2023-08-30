<script>
export default {
  data() {
    return {
      error: '',
      valueInput: '',
      editingtask: false,
      Dolist: [],
      Completelist: []
    };
  },

  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },

  methods: {
    hanlyInput(event) {
      this.valueInput = event.target.value;
    },
    addTask() {
      if (this.valueInput.trim().length < 2) {
        this.error = "Error";
        return;
      };
      this.Dolist.push({
        title: this.valueInput,
        id: Math.random()
      });
      this.valueInput = '';
      this.error = '';
    },
    doCheck(index, type) {

      if (type === 'need') {
        const completeMask = this.Dolist.splice(index, 1);
        this.Completelist.push(...completeMask);
      }

      else {
        const noCompleteMask = this.Completelist.splice(index, 1);
        this.Dolist.push(...noCompleteMask);
      }
    },



    editTodoTask() {
      this.Dolist.editingtask = true;
    },

    doneEditTask() {
      this.Dolist.editingtask = false;

    },

    remove(index, type) {
      const toDO = type === 'need' ? this.Dolist : this.Completelist;
      toDO.splice(index, 1);
    }
  }
}
</script>

<template>
  <div class="container">
    <header>
      <div class="title">ToDoList</div>
      <div class="send_menu">
        <input class="window" v-bind:value="valueInput" v-on:input="hanlyInput" v-on:keypress.enter="addTask" type="text"
          placeholder="Enter task">
        <button class="send" v-on:click="addTask">Add</button>
      </div>
      <div class="error">{{ error }}</div>
    </header>
    <main class="main">
      <div class="block_task">
        <div class="title_task">Tasks</div>
        <div class="numbers_task">{{ Dolist.length }}</div>
      </div>
      <ul class="mask-list">
        <li v-for="(mask, index) in  Dolist " :key="mask.id">
          <label class="cont_check">
            <input type="checkbox" v-on:change="doCheck(index, 'need')">
            <div class="checkmark"></div>
          </label>
          <div class="result" v-if="!Dolist.editingtask" @dblclick="editTodoTask()">{{ mask.title }}</div>
          <div v-else class="item-edit"><input class="editTask" type="text" v-model="mask.title" @blur="doneEditTask()"
              @keyup.enter="doneEditTask()" v-focus></div>
          <button class="delete" v-on:click="remove(index, 'need')"><svg xmlns="http://www.w3.org/2000/svg" width="24"
              height="24" viewBox="0 0 24 24" style="fill: rgba(0, 0, 0, 1);transform: ;msFilter:;">
              <path
                d="M6 7H5v13a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V7H6zm4 12H8v-9h2v9zm6 0h-2v-9h2v9zm.618-15L15 2H9L7.382 4H3v2h18V4z">
              </path>
            </svg></button>
        </li>
      </ul>
      <div class="block_done">
        <div class="title_done">Done</div>
        <div class="numbers_done">{{ Completelist.length }}</div>
      </div>
      <ul class="mask_list complete_list">
        <li v-for="(mask, index) in Completelist" :key="mask.id">
          <label class="cont_check">
            <input checked="checked" type="checkbox" v-on:change="doCheck(index, 'complete')">
            <div class="checkmark"></div>
          </label>
          <div class="block-task_input">
            <div class="resultdone"> {{ mask.title }} </div>
          </div>
          <button class="delete" v-on:click="remove(index, 'complete')"><svg xmlns="http://www.w3.org/2000/svg" width="24"
              height="24" viewBox="0 0 24 24" style="fill: rgba(0, 0, 0, 1);transform: ;msFilter:;">
              <path
                d="M6 7H5v13a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V7H6zm4 12H8v-9h2v9zm6 0h-2v-9h2v9zm.618-15L15 2H9L7.382 4H3v2h18V4z">
              </path>
            </svg>
          </button>
        </li>
      </ul>
    </main>
  </div>
</template>

<style scoped lang="scss">
@import'./assets/scss/main.scss';

.container {
  max-width: 1440px;
  width: 100%;
  padding: 20px;
}

.title {
  text-align: center;
  color: $black;
  font-size: 50px;
  font-weight: 700;

  &_task {
    color: $dark-grey;
    font-size: 2px;
  }

  &_done {
    color: $dark-grey;
    font-size: 20px;
  }
}

.send_menu {
  margin-top: 20px;
  display: flex;
  justify-items: center;
  gap: 2%;
}

.window {
  display: flex;
  width: 100%;
  max-width: 260px;
  padding: 10px;
  align-items: center;
  color: $dark-grey;
  background: $grey;
  font-size: calc(15px + 15 * (100vw / 1440));
  font-weight: 400;
  border: none;
  border-radius: 10px;
  outline: none;
  @include transition(color);

  &:focus {
    color: $black;
  }
}

li {
  padding: 10px;
  display: grid;
  grid-template-columns: repeat(3, max-content);
  align-items: center;
  gap: 10px;
}



.send {
  width: 100%;
  max-width: 80px;
  color: $black;
  background: $grey;
  font-size: calc(15px + 15 * (100vw / 1440));
  font-weight: 400;
  border: none;
  border-radius: 10px;
  @include transition (transform);

  &:hover {
    transform: scale(1.05);
  }

  &:active {
    transform: scale(1);
  }
}

.error {
  margin-top: 20px;
  font-weight: 500;
  font-size: 20px;
  text-align: center;
  color: $red;
}

.main {
  width: 100%;
  max-width: 400px;
  margin-top: 30px;
  padding: 30px;
  background-color: $grey;
  display: grid;
  gap: 20px;
  border-radius: 20px;
}

.block_task,
.block_done {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.title_task,
.title_done {
  font-size: 20px;
}

.delete {
  border: none;
  background: #00000000;
  cursor: pointer;
  @include transition(transform);

  &:hover {
    transform: scale(1.05);
  }

  &:active {
    transform: scale(1);
  }
}

.editTask {
  background-color: $white;
  border-radius: 20px;
  padding: 5px;
  border: none;
  font-size: 15px;
}

.result {
  max-width: 200px;
  height: max-content;
}

.resultdone {
  max-width: 200px;
  height: max-content;
}
</style>
