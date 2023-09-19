<template>
  <main>
    <div class="list-controls">
      <button class="list-button" @click="clearList()">
        Download TODO list
      </button>
      <input
        type="text"
        class="list-input"
        ref="input"
        placeholder="Write the todo item"
        v-model="searchText"
        @keyup.enter="addItem(); inputFocus = false;"
      />
      <p class="list-input-description">
        Press "Enter" to add the new ToDo item to the list
      </p>
    </div>
    <div class="list-container">
      <ul class="list-item-container">
        <li class="list-item" v-for="(item, index) in posts" :key="index + '1'">
          <span v-if="item && item.title"
            >{{ `${index + 1}.` }}
            <span :class="{ 'list-item-active': index > 9 }">{{
              item.title
            }}</span></span
          >
        </li>
        <p class="list-item-description" v-if="inputFocus">Typing...</p>
      </ul>
      <ul class="list-item-container">
        <li class="list-item" v-for="(item, index) in posts" :key="index + '2'">
          <span v-if="item && item.title"
            >{{ `${index + 1}.` }}
            <span :class="{ 'list-item-active': index > 9 }">{{
              item.title
            }}</span></span
          >
        </li>
        <p class="list-item-description" v-if="inputFocus">Typing...</p>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import { useFocus } from "@vueuse/core";

const res = ref(await fetch("https://jsonplaceholder.typicode.com/todos"));
const posts = ref((await res.value.json()).slice(0, 10));

const input = ref();
const searchText = ref("");

const { focused: inputFocus } = useFocus(input);

function clearList() {
  posts.value = posts.value.slice(0, 10);
}

function addItem() {
  posts.value.push({ title: searchText.value });
  searchText.value = "";
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
ul {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

p {
  margin: 0;
}

.list {
  &-controls {
    margin-bottom: 15px;
  }
  &-button {
    display: block;
    margin-bottom: 5px;
  }
  &-input {
    margin-bottom: 5px;
  }
  &-container {
    display: flex;
    flex-wrap: wrap;
    gap: 30px;
  }
  &-item {
    &-description {
      margin-top: 10px;
      color: #bdbdbd;
    }
    &-active {
      color: #66bb6a;
    }
  }
}
</style>
