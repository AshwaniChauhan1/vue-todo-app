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
        <v-card-title>
          <span class="headline">Add Task</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-text-field label="Enter todo" v-model="form.task"></v-text-field>
            <v-text-field label="Date" type="date" v-model="form.date"></v-text-field>
            <v-text-field label="Time" type="time" value="12:00:00" v-model="form.time"></v-text-field>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="close">Close</v-btn>
          <v-btn color="blue darken-1" text @click="save">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-card class="mx-8">
      <v-card-title class="blue-grey lighten-1 white--text">Task To Do :</v-card-title>
        <div class="d-flex">
          <div class="py-5" v-for="(task,index) in tasks" :key="index">
              <v-card class="my-5 mx-5">
                <v-list-item class="d-flex justify-space-between font-weight-bold title">
                  {{ task.task }}
                  <v-checkbox class="mt-5 ml-3" v-model="form.check" @change="onclick"></v-checkbox>
                </v-list-item>
                <v-list-item v-if="task.complete"><p class="red--text">Completed</p></v-list-item>
                <v-list-item>
                  <v-list-item-content>Date : {{task.date}}</v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>Time : {{task.time}}</v-list-item-content>
                </v-list-item>
                <v-list-item class="d-flex justify-space-between">
                  <div>
                    <a class="float-left" @click="edit(task,index)">Edit</a>
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
      form: { task: "", date: "", time: "" ,check:""},
      val: {},
      tasks: [],
      editIndex: false,
    };
  },
  methods: {
    save() {
      if (this.editIndex == true) {
        this.val = {
          task: this.form.task,
          date: this.form.date,
          time: this.form.time,
          check: this.form.check
        };
        this.tasks.splice(this.indexval, 1, this.val);
        this.dialog = false;
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
    },
    close() {
      this.dialog = false;
      this.clear();
    },
    del(index) {
      this.$delete(this.tasks, index);
    },
    edit(task, index) {
      this.dialog = true;
      this.form.task = task.task;
      this.form.date = task.date;
      this.form.time = task.time;
      this.editIndex = true;
      this.indexval = index;
    },
    clear() {
      this.form = { task: "", date: "", time: "", check: "" };
    },
    onclick(task){
      if(task.check == true){
        this.form.check= false;
        //eslint-disable-next-line
      console.log("true");
      }else{
        //eslint-disable-next-line
        console.log("false");
      }
    }
  }
};
</script>