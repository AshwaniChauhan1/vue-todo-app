<template>
  <div>
    <div class="pa-8 blue-grey darken-2 white--text">
      <h1 class="text-center">TODO - APP</h1>
    </div>
    <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:activator="{ on }">
        <v-btn class="ma-8" color="primary" dark v-on="on">
          <p class="mb-0">Add Task</p>
          <v-icon dark>mdi-plus</v-icon>
        </v-btn>
      </template>
      <v-card>
        <v-card-text>
          <v-container>
            <v-text-field label="Enter Task Here..." v-model="form.task"></v-text-field>
            <div class="d-flex justify-space-between">
              <v-date-picker v-model="form.date" width="220"></v-date-picker>
              <v-time-picker width="220" v-model="form.time" v-if="dialog"></v-time-picker>
            </div>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-card-text class="red--text">
            <p>{{error}}</p>
          </v-card-text>
          <v-btn color="blue darken-1" text @click="close">Close</v-btn>
          <v-btn color="blue darken-1" text @click="save">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-card class="mx-8">
      <v-card-title class="blue-grey lighten-1 white--text">Task To Do :</v-card-title>
      <div class="d-flex flex-wrap">
        <div class="py-5" v-for="(task,index) in tasks" :key="index">
          <v-card class="my-5 mx-5">
            <v-list-item class="d-flex justify-space-between font-weight-bold title">
              <p class="red--text" v-if="task.check">
                <del>{{ task.task }}</del>
              </p>
              <p v-if="!task.check">{{ task.task }}</p>
              <v-checkbox class="mt-2 ml-3" v-model="task.check"></v-checkbox>
            </v-list-item>
            <v-list-item>
              <v-list-item-content>Date : {{task.date}}</v-list-item-content>
            </v-list-item>
            <v-list-item>
              <v-list-item-content>Time : {{task.time}}</v-list-item-content>
            </v-list-item>
            <v-list-item class="d-flex justify-space-between">
              <div>
                <a class="float-left" @click="edit(index)">Edit</a>
              </div>
              <div>
                <a class="float-left" @click="del(index)">Delete</a>
              </div>
            </v-list-item>
          </v-card>
        </div>
      </div>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "todo",
  data: () => {
    return {
      dialog: false,
      picker: null,
      form: { task: "", date: "", time: "", check: false },
      val: {},
      tasks: [],
      editIndex: false,
      error: ""
    };
  },
  methods: {
    save() {
      if (this.form.task == "") {
        this.error = "* Task Required";
      } else if (this.form.date == "") {
        this.error = "* Date Required";
      } else if (this.form.time == "") {
        this.error = "* Time Required";
      } else {
        if (this.editIndex == true) {
          this.val = {
            task: this.form.task,
            date: this.form.date,
            time: this.form.time,
            check: this.form.check
          };
          this.tasks.splice(this.indexval, 1, this.val);
          this.dialog = false;
          this.editIndex = false;
          this.clear();
        } else {
          this.val = {
            task: this.form.task,
            date: this.form.date,
            time: this.form.time,
            check: this.form.check
          };
          this.tasks.push(this.val);
          this.dialog = false;
          this.clear();
        }
      }
    },
    close() {
      this.dialog = false;
      this.clear();
    },
    del(index) {
      this.$delete(this.tasks, index);
    },
    edit(index) {
      this.dialog = true;
      this.form.task = this.tasks[index].task;
      this.form.date = this.tasks[index].date;
      this.form.time = this.tasks[index].time;
      this.form.check = this.tasks[index].check;
      this.editIndex = true;
      this.indexval = index;
    },
    clear() {
      this.form = { task: "", date: "", time: "", check: false };
      this.error = "";
    }
  }
};
</script>