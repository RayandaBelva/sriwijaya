<template>
  <div>
    <h1 class="mt-5 text-center mb-3">Todos Data</h1>
    <li class="nav-item active">
      <button href="Add.vue" type="button" class="btn btn-primary">Add</button>
    </li>
    <table class="table">
      <thead>
        <tr class="header">
          <th>ID</th>
          <th>Title</th>
          <th>Completed</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list_paged" :key="item.id" class="item">
          <td>{{ item.id }}</td>
          <td>{{ item.title }}</td>
          <td>{{ item.completed }}</td>
          <td>
            <button href="App.vue" type="button" class="btn btn-primary">Update</button>
            <button href="App.vue" type="button" class="btn btn-danger">Delete</button>
          </td>
        </tr>
      </tbody>
      <div class="column">
        <p>
          Data <b>{{ begin+1 }} - {{ end >= list.length ? list.length : end }}</b>
          from <b>{{ list.length }}</b> data
        </p>
        <div class="column has-text-right">
          <button class="bi bi-arrow-left" @click="pageNav('left')" :disabled="begin===0">◀</button>
          <button class="bi bi-arrow-right" @click="pageNav('right')" :disabled="end >= list.length">▶</button>
        </div>
      </div>
    </table>
  </div>
</template>

<script>
import axios from "axios"
export default {
  name: "App",
  data() {
    return {
      list: [],
      list_paged: [],
      begin: 0,
      end: 10
    }
  },

  async mounted() {
    let result = await axios.get("https://jsonplaceholder.typicode.com/todos/");
    this.list = result.data;
    this.pageNav(); 
  },
  
  methods: {
    pageNav(direction) {
      if (direction === 'left' && this.begin > 0) {
        this.begin -= 10;
        this.end -= 10;
      } else if (direction === 'right' && this.end < this.list.length) {
        this.begin += 10;
        this.end += 10;
      }
      this.list_paged = this.list.slice(this.begin, this.end);
    }
  }
};
</script>

<style>
.table {
  width: 100%;
  border-collapse: collapse;
}

.header th,
.item td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.header th {
  background-color: #f2f2f2;
}
</style>
