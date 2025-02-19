<template>
  <div class="row mt-1" style="height: 400px">
    <div class="col-10 offset-1 bg-light rounded-3 shadow-lg">
      <div class="text-center mt-2">
        <h4 class="text-danger">
          <i class="fa-regular fa-clipboard"></i> {{ title }}
        </h4>
      </div>
      <div class="col-12 col-md-8 offset-md-2 d-flex mt-3">
        <input
          type="text"
          class="form-control w-100 me-2"
          placeholder="New Task...."
          v-model="mytask"
          v-on:keyup.enter="add()"
        />
        <button
          class="form-control w-25"
          style="background-color: rgb(161, 175, 175)"
          @click="add()"
        >
          <span style="color: rgb(22, 218, 218)" class="fs-6 fs-md-5 fs-lg-4"
            >ADD</span
          >
        </button>
      </div>
      <div
        class="d-flex align-items-center justify-content-center my-4"
        v-if="Tasks.length == 0"
      >
        <span class="fs-5">Start your first task !</span>
      </div>
      <div
        class="col-12 col-md-8 offset-md-2 mt-3 d-flex justify-content-between"
        v-for="(item, index) in Tasks"
        :key="index"
      >
        <div>
          <input
            type="checkbox"
            class="mx-2 rounded-3"
            v-model="item.done"
            @change="checkboxstatus()"
          />
          <span
            :class="item.done == true ? 'text-decoration-line-through' : ''"
            >{{ item.action }}</span
          >
        </div>

        <div>
          <button
            class="btn btn-outline-danger btn-sm"
            @click="deleteEachtasks(item.id)"
          >
            <i class="fa-solid fa-trash"></i>
          </button>
        </div>
      </div>
      <div class="row">
        <div class="col-10 offset-1 d-flex justify-content-between mb-2">
          <button
            class="btn-sm btn btn-outline-danger mt-2 rounded-3 col-6 col-md-5 me-sm-3"
            @click="complete()"
          >
            Clear <span class="d-md-inline d-sm-block">completed</span>
          </button>
          <button
            class="btn-sm btn btn-outline-danger mt-2 rounded-3 col-6 col-md-5"
            @click="all()"
          >
            Clear <span class="d-md-inline d-sm-block">all</span>
          </button>
        </div>
        <div class="mt-2" v-if="Tasks.length != 0">
          Pending Tasks : {{ pendingTasks }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomeView",
  data() {
    return {
      title: "What's your daily plan?",
      mytask: "",
      Tasks: [],
      done: false,
    };
  },
  computed: {
    pendingTasks() {
      return this.Tasks.filter((v) => !v.done).length;
    },
  },
  methods: {
    add() {
      if (this.mytask != "") {
        this.Tasks.push({
          id: this.Tasks.length + 1,
          action: this.mytask,
          done: false,
        });

        this.storeData();
        this.Pendingtaskcount = this.Tasks.length;
        this.mytask = [];
      }
    },
    storeData() {
      localStorage.setItem("MyTasks", JSON.stringify(this.Tasks));
    },
    deleteEachtasks(id) {
      this.Tasks = this.Tasks.filter((v) => v.id != id);
    },
    complete() {
      this.Tasks = this.Tasks.filter((v) => !v.done);
      this.storeData();
    },
    all() {
      this.Tasks = this.Tasks.filter((v) => !v.done && v.done);
      this.storeData();
    },
    checkboxstatus() {
      this.storeData();
    },
  },
  created() {
    let data = JSON.parse(localStorage.getItem("MyTasks")) || [];
    this.Tasks = data;
  },
};
</script>

<style lang="stylus" scoped></style>
