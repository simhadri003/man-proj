<template>
  <div class="task">
    <div class="task-fp">
      <h3>
        {{ match.id }}
      </h3>

      <h3>
        {{ match.match }}
      </h3>

      <h3>
        {{ match.level }}
      </h3>

      <div :class="[header ? 'true' : 'match-header', '']">
      <div :key="refer" v-for="refer in match.refrees">
        <h3>{{ refer }}</h3>
      </div>
    </div>

      <div v-if="match.level === match.refrees.length">
        <!-- <i @click="$emit('delete-task', task.id)" class="fas fa-edit"></i> -->
        <i class="fas fa-check-circle"></i>
      </div>
      <div v-else>
        <i @click="toggleShowRefDetail()" class="fas fa-edit"></i>
      </div>
      <!-- <p>{{ task.id }}</p> -->
      <!-- <div>
      <b-button v-b-modal.modal-1>Launch demo modal</b-button>

      <b-modal id="modal-1" title="BootstrapVue">
        <p class="my-4">Hello from modal!</p>
      </b-modal>
    </div> -->
    </div>
    <div v-if="showRefDetails" class="ref-det">
      <!-- <i @click="$emit('delete-task', task.id)" class="fas fa-edit"></i> -->
      <h4>Refree deatils</h4>
      <h6>
        ** {{ match.refrees.length }} refs have been assigned alreaady, assign
        {{ match.level - match.refrees.length }} more
      </h6>
      <div>
        <div :key="refer" v-for="refer in match.refrees">
          <i
            ><h4 class="ref-left">{{ refer }}</h4></i
          >
        </div>
        <div>
          <!-- <div :key="n" v-for="n in match.level - match.refrees.length">
            <input placeholder="Enter Refree Name" />
          </div> -->
          <textarea :value="refs" @input="event => updateRefrees(event, match.level - match.refrees.length)" name="Text1" cols="40" rows="5" placeholder="Enter Refree names using comma separtor"></textarea>
        </div>


              <div v-if="isValidRefInput">
        <Button 
      @btn-click="() => emitFunction(match.id, this.refs)"
      text="Update"
      color="green" />
      </div>

      </div>
    </div>
  </div>
</template>

<script>
import Button from './Button'
export default {
  name: "Task",
  props: {
    match: Object,
    header: String,
  },
  components: {
    Button
  },
  data() {
    return {
      showRefDetails: false,
      refs: '',
      isValidRefInput: false
    };
  },
  methods: {
    toggleShowRefDetail() {
      console.log("showRefDetails", this.showRefDetails);
      this.showRefDetails = !this.showRefDetails;
    },
    updateRefrees(event, cntr) {
      this.refs = event.target.value
      if ((event.target.value).split(',').length !==  cntr){
        this.isValidRefInput = false
      }
      else {
        this.isValidRefInput = true
      }
    console.log("refs", this.refs)
    },
    emitFunction(id, refs) {
      this.$parent.$emit('update-ref-det', id, refs)
      console.log("hello")
      this.showRefDetails = false
    }
  },
};
</script>

<style scope>
.ref-det {
  padding-top: 20px;
}
.ref-left {
  justify-content: flex-start !important;
}
.refree-list {
  display: flex;
  flex-direction: column;
}
.fa-edit {
  color: red;
  font-size: 20px;
  cursor: pointer;
}

.fa-check-circle {
  color: green;
  font-size: 20px;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
}

.task-fp {
  /* cursor: pointer; */
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-around;
  color: #444444;
}
</style>
