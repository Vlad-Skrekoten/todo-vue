<script>
export default {
  data() {
    return {
      valueInput: '',
      Dolist: [],
      Completelist: []
    };
  },

  methods: {
    hanlyInput(event) {
      this.valueInput = event.target.value;
    },
    addTask() {
      if (this.valueInput === '') { return };
      this.Dolist.push({
        title: this.valueInput,
        id: Math.random()
      });
      this.valueInput = '';
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
    }
  }
}
</script>

<template>
  <div class="container">
    <header>
      <div class="title">All Tasks</div>
      <div class="send_menu">
        <input class="window" v-bind:value="valueInput" v-on:input="hanlyInput" v-on:keypress.enter="addTask" type="text"
          placeholder="Enter task">
        <button class="send" v-on:click="addTask">Add</button>
      </div>
    </header>
    <main>
      <div class="title_task">Task</div>
      <div class="window_task">
        <ul class="mask-list">
          <li v-for="(mask, index) in  Dolist " :key="mask.id">
            <label class="cont_check">
              <input type="checkbox" v-on:change="doCheck(index, 'need')">
              <div class="checkmark"></div>
            </label>
            <span>{{ mask.title }}</span>
          </li>
        </ul>
      </div>
      <div class="title_done">Done</div>
      <ul class="mask_list complete_list">
        <li v-for="(mask, index) in Completelist" :key="mask.id">
          <div class="window_done">
            <label class="cont_check">
              <input checked="checked" type="checkbox" v-on:change="doCheck(index, 'complete')">
              <div class="checkmark"></div>
            </label>
            <span>{{ mask.title }}</span>
          </div>
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
  grid-template-columns: repeat(2, max-content);
  align-items: center;
  gap: 10px;
}

.send {
  width:100%;
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

main {
  margin-top: 30px;
  padding: 30px;
  background-color: $grey;
  display: grid;
  gap: 20px;
  border-radius: 20px;
}

.title_task,
.title_done {
  font-size: 20px;
}

.window_task,
.window_done {
  display: grid;
  grid-template-columns: repeat(2, max-content);
  justify-content: start;
  align-items: center;
  gap: 10px;
}

span.check {
  text-decoration: line-through 2px solid $red;
}
</style>
