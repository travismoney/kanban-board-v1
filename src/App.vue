<template>
  <div class="container-lg mt-5">
    <!-- Heading -->
    <h3 class="font-color">Kanban Board V1. 
      <b-button class="btn btn-success btn-circle btn-xl float-right shadow-lg btn-add" v-b-modal.modal-prevent-closing><i class="fas fa-plus"></i></b-button>
      <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Add New Task"
        @show="resetModal"
        @hidden="resetModal"
        @ok="handleOk"
      >
        <form ref="form" @submit.stop.prevent="handleSubmit">
          <b-form-group
            label="Task"
            label-for="task-input"
            invalid-feedback="Please Add Task!"
            :state="taskState"
          >
            <b-form-input
              id="task-input"
              v-model="newTask"
              :state="taskState"
              required
            >
            </b-form-input>
          </b-form-group>
        </form>
      </b-modal>
   </h3>
    <div class="row mt-5">
    <!-- Column Backlog Start -->
      <div class="col-3">
        <div class="p-3 card rounded-corners bg-custom-red border-0 shadow-box ">
          <h5 class="p-2 font-color text-white">Backlog</h5>
          <draggable
            class="list-group kanban-column"
            :list="arrBacklog"
            group="tasks"
          >
            <div
              class="list-group-item font-color mb-3 shadow-box border-0 rounded-corners"
              v-for="(element,index) in arrBacklog"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromBacklog(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>
      <!-- Column Backlog End -->
      <!-- Column In Progress Start -->
      <div class="col-3">
        <div class="p-3 card rounded-corners bg-custom-orange border-0 shadow-box">
          <h5 class="p-2 font-color text-white">In Progress</h5>
          <draggable
            class="list-group kanban-column"
            :list="arrInProgress"
            group="tasks"
          >
            <div
              class="list-group-item font-color mb-3 shadow-box border-0 rounded-corners"
              v-for="(element,index) in arrInProgress"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromInProgress(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>
      <!-- Column In Progress End -->
      <!-- Column In Review Start -->
      <div class="col-3">
        <div class="p-3 card rounded-corners bg-custom-blue border-0 shadow-box">
          <h5 class="p-2 font-color text-white">In Review</h5>
          <draggable
            class="list-group kanban-column"
            :list="arrInReview"
            group="tasks"
          >
            <div
              class="list-group-item font-color mb-3 shadow-box border-0 rounded-corners"
              v-for="(element,index) in arrInReview"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromInReview(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>
      <!-- Column In Review End -->
      <!-- Column Completed Start -->
      <div class="col-3">
        <div class="p-3 card rounded-corners bg-custom-green border-0 shadow-box">
          <h5 class="p-2 font-color text-white">Completed</h5>
          <!-- Backlog draggable component. Pass arrBackLog to list prop -->
          <draggable
            class="list-group kanban-column"
            :list="arrCompleted"
            group="tasks"
          >
            <div
              class="list-group-item font-color mb-3 shadow-box border-0 rounded-corners"
              v-for="(element,index) in arrCompleted"
              :key="element.name"
            >
              <div class="font-small">
                {{ element.name }}
                <i class="far fa-times-circle float-right delete-circle" @click="removeFromCompleted(index)"></i>
              </div>
            </div>
          </draggable>
        </div>
      </div>
      <!-- Column Completed End -->
    </div>
  </div>
</template>

<script>

// import draggable
import draggable from "vuedraggable";

export default {

  name: "kanban-board",

  components: {
    // import draggable as a component
    draggable
  },

  data() {
    return {
      // variables for new tasks
      newTask: "",
      taskState: null,

      // Initialized first arrray with some data
      arrBacklog: [
        { name: "Debug Scroll Issue" },
        { name: "Design Wireframe" },
        { name: "Prepare SRS" },
        { name: "Prepare SDS" },
        { name: "Test System" }
      ],

      // Empty arrays
      arrInProgress: [],
      arrInReview: [],
      arrCompleted: []

    };

  },
  
  methods: {

    // check form is filled 
    checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.taskState = valid
        return valid
      },

    // reset input fields
    resetModal() {
      this.newTask = "";
      this.taskState = "";
    },

    handleOk(bvModalEvt) {
      // Prevent modal from closing
      bvModalEvt.preventDefault()
      // Trigger submit handler
      this.handleSubmit()
    },

    handleSubmit() {
      // Exit when the form isn't valid
      if (!this.checkFormValidity()) {
        return
      }
      // Push the push new tasks to arrBacklog
        this.arrBacklog.push({ name: this.newTask });
        this.newTask = "";
        // Hide the modal manually
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
      },

    // delete task from backlog array
    removeFromBacklog(id){
      this.$delete(this.arrBacklog, id);
    },

    // delete task from in progress array
    removeFromInProgress(id){
      this.$delete(this.arrInProgress, id);
    },

    // delete task from in review array
    removeFromInReview(id){
      this.$delete(this.arrInReview, id);
    },

    // delete task from completed array
    removeFromCompleted(id){
      this.$delete(this.arrCompleted, id);
    }
  }
};
</script>

<style>
/* import styles.css */
@import './assets/styles/styles.css';
</style>
