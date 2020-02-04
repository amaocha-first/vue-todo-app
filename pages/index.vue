<template>
  <section class="container">
    <h1>Todo App</h1>
    <div class="input-container">
      <label for="text" id="text">todo</label>
      <br />
      <input
        id="text"
        type="text"
        name="content"
        autocomplete="off"
        v-model="content"
        @focus="set_flg"
        placeholder="input your task!"
      />
      <div class="text_underline"></div>
    </div>
    <button id="save" @click="insertItem">追加</button>
    <button id="find" @click="find">検索</button>
    <div class="items-container">
      <ul>
        <li class="item" v-for="(todo, index) in display_todos" :key="index">
          <div id="item-content">
            <span id="remove" @click="removeItem(todo)">×</span>
            <span>{{ todo.content }}</span>
            <br />
            <span>({{ todo.created }})</span>
          </div>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import { mapState } from "vuex";

export default {
  data: function() {
    return {
      content: "",
      find_flg: false
    };
  },
  computed: {
    ...mapState(["todos"]),
    display_todos: function() {
      if (this.find_flg) {
        var arr = [];
        var data = this.todos;
        data.forEach(element => {
          if (element.content.toLowerCase() == this.content.toLowerCase()) {
            arr.push(element);
          }
        });
        return arr;
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    insertItem: function() {
      this.$store.commit("insert", { content: this.content });
      this.content = "";
    },
    find: function() {
      this.find_flg = true;
    },
    set_flg: function() {
      if (this.find_flg) {
        this.find_flg = false;
        this.content = "";
      }
    },
    removeItem: function(todo) {
      this.$store.commit("remove", todo);
    }
  }
};
</script>
