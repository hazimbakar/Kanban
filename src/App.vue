<template>
  <div class="container-fluid">
    <h1 class="d-flex justify-content-center bg-dark text-white p-3">My Todo List</h1>
    <b-container class="mt-0">
      <b-row class="my-0">
        <b-col class="col-md-4">
          <b-card class="mb-3" style="border: none;">
            <b-row>
              <b-col class="col-md-8 d-flex align-items-center">
                <h3>Task List</h3>
              </b-col>

              <b-modal id="modal-1" size="lg" centered title="New todo (New List)">
                <p class="my-4">Enter your new todo: </p>
                <input-todo @new-todo="submitNewTodo"></input-todo>
                <template #modal-footer>
                  <b-button variant="danger" @click="hideModal">Close</b-button>
                </template>
              </b-modal>

              <b-col class="col-md-4">
                <b-button style="border: none;" class="bg-white" @click="openModal">
                  <b-icon variant="dark" icon="file-plus" font-scale="1"></b-icon>
                </b-button>
              </b-col>
            </b-row>
            
          </b-card>

          <draggable class="list-group" :list="newTaskList" group="tasks">
            <div class="list-group-item" v-for="elem in newTaskList" :key="elem.task">
              {{ elem.task }}
              <b-row class="mt-3">
                <b-col class="pb-2">
                  <b-button variant="outline-primary" style="border: 0px;" size="sm" @click="popUp(elem)">Edit</b-button>
                </b-col>
                <b-col class="pb-2">
                  <b-button variant="outline-primary" style="border: 0px;" size="sm"
                    @click="deleteTodo(elem)">Delete</b-button>
                </b-col>

                <div v-if="popupVisible && activeTask === elem" class="col form-inline">
                  <b-form-input v-model="edit" placeholder="What to edit..." class="mb-2"></b-form-input>
                  <b-button size="sm" @click="editedTodo(elem)">Submit Edit</b-button>
                  <b-button size="sm" class="ml-2" @click="closePopup(elem)">Cancel</b-button>
                </div>

              </b-row>
            </div>
          </draggable>
        </b-col>

        <b-col class="col-md-4">
          <b-card class="mb-3" style="border: none;">
            <b-row>
              <b-col class="col-md-8 d-flex align-items-center">
                <h3>In Progress</h3>
              </b-col>

              <b-modal id="modal-2" size="lg" centered title="New todo (In Progress)">
                <p class="my-4">Enter your new todo: </p>
                <input-todo @new-todo="submitNewTodoInProgress"></input-todo>
                <template #modal-footer>
                  <b-button variant="danger" @click="hideModalInProgress">Close</b-button>
                </template>
              </b-modal>

              <b-col class="col-md-4">
                <b-button style="border: none;" class="bg-white" @click="openModalInProgress">
                  <b-icon variant="dark" icon="file-plus" font-scale="1"></b-icon>
                </b-button>
              </b-col>
            </b-row>
              

              
       
          </b-card>
          <draggable class="list-group" :list="inprogressTask" group="tasks">
            <div class="list-group-item" v-for="elem in inprogressTask" :key="elem.task">
              {{ elem.task }}
            </div>

          </draggable>
        </b-col>

        <b-col class="col-md-4">
          <b-card class="mb-3" style="border: none;">
            <b-row>
              <b-col class="col-md-8 d-flex align-items-center">
                <h3>Completed</h3>
              </b-col>

              <b-modal id="modal-3" size="lg" centered title="New todo (Completed)">
                <p class="my-4">Enter your new todo: </p>
                <input-todo @new-todo="submitNewTodoCompleted"></input-todo>
                <template #modal-footer>
                  <b-button variant="danger" @click="hideModalCompleted">Close</b-button>
                </template>
              </b-modal>

              <b-col class="col-md-4">
                <b-button style="border: none;" class="bg-white" @click="openModalCompleted">
                  <b-icon variant="dark" icon="file-plus" font-scale="1"></b-icon>
                </b-button>
              </b-col>

            </b-row>
          </b-card>
          <draggable class="list-group" :list="completedTask" group="tasks">

            <b-row class="ml-0 mr-0 list-group-item" v-for="elem in completedTask" :key="elem.task">
              <b-row>
                <b-col class="col-md-8">
                  {{ elem.task }}
                </b-col>
                <b-button class="bg-white" style="border: none;" @click="deleteCompleted(elem)"><b-icon class="col-md-1"
                    variant="dark" icon="x" font-scale="1"></b-icon></b-button>
              </b-row>
            </b-row>
          </draggable>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import InputTodo from "./components/InputTodo.vue";

export default {
  name: 'App',
  components: {
    draggable,
    InputTodo
  },
  data() {
    return {
      newTask: "",
      noPopup: false,
      noPopupInProgress: false,
      noPopupCompleted: false,
      newTaskList: [
        { task: "play" },
        { task: "eat" },
        { task: "learn" }
      ],
      inprogressTask: [],
      completedTask: [
        { task: "play" },
        { task: "play234S" }
      ],
      popupVisible: false,
      activeTask: null,
      edited: "",
      edit: ""
    }
  },
  methods: {
    hideModal(){
      this.$bvModal.hide('modal-1')
    },
    hideModalInProgress(){
      this.$bvModal.hide('modal-2')
    },
    hideModalCompleted(){
      this.$bvModal.hide('modal-3')
    },
    submitNewTodo(list) {
      const newTodo = {
        task: list
      }
      this.newTaskList.push(newTodo);
      this.hideModal()
    },
    submitNewTodoPopup() {
      this.noPopup = !this.noPopup
    },
    submitNewTodoInProgress(list) {
      const newTodo = {
        task: list
      }
      this.inprogressTask.push(newTodo);
      this.hideModalInProgress()
    },
    submitNewTodoInProgressPopup() {
      this.noPopupInProgress = !this.noPopupInProgress
    },
    submitNewTodoCompleted(list) {
      const newTodo = {
        task: list
      }
      this.completedTask.push(newTodo);
      this.hideModalCompleted()
    },
    submitNewTodoCompletedPopup() {
      this.noPopupCompleted = !this.noPopupCompleted
    },
    popUp(task) {
      if (this.popupVisible === false) {
        this.popupVisible = true;
        this.activeTask = task;
      } else {
        this.popupVisible = false;
        this.activeTask = task;
      }
    },
    closePopup(task) {
      if (this.popupVisible === false) {
        this.popupVisible = true;
        this.activeTask = task;
      } else {
        this.popupVisible = false;
        this.activeTask = task;
      }
    },
    deleteTodo(task) {
      this.newTaskList = this.newTaskList.filter(
        (item) => item !== task
      );
    },
    editedTodo(task) {
      const updatedTask = { ...task, task: this.edit };
      const index = this.newTaskList.findIndex((item) => item === task);
      if (index !== -1) {
        this.newTaskList.splice(index, 1, updatedTask);
      }
      this.popupVisible = false;
      this.activeTask = null;
      this.edit = "";
    },
    deleteCompleted(task) {
      this.completedTask = this.completedTask.filter(
        (item) => item !== task
      );
      console.log('test')
    },
    openModal(){
      this.$bvModal.show('modal-1')
    },
    openModalInProgress(){
      this.$bvModal.show('modal-2')
    },
    openModalCompleted(){
      this.$bvModal.show('modal-3')
    }
  }
}
</script>
