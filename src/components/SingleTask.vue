<template>
  <div class="project" :class="{ complete: task.complete }">
    <div class="actions">
      <h3 @click="showDetails = !showDetails">{{ task.title }}</h3>
      <div class="icons">
        <span class="material-icons" @click="handleDelete">delete</span>
        <span class="material-icons" >edit</span>
        <span @click="toggleComplete" class="material-icons tick">done</span>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p class="details">{{ task.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["task"],
  data() {
    return {
      showDetails: false,
      uri: 'http://localhost:3000/tasks/' + this.task.id
    };
  },
  methods: {
      handleDelete() {
          fetch(this.uri, {method: 'DELETE'})
          .then(() => this.$emit('delete', this.task.id))
          .catch(err => console.log(err))
      },
      toggleComplete() {
          fetch(this.uri, {
              method: 'PATCH',
              headers: {'Content-Type': 'application/json'},
              body: JSON.stringify({complete: !this.task.complete})
              })
          .then(() => this.$emit('complete', this.task.id))
          .catch(err => console.log(err))
      }
  }
};
</script>
    
<style>
.project {
  background-color: rgb(238, 238, 238);
  border-radius: 5px;
  padding: 10px 25px;
  margin-bottom: 25px;
  border-left: 4px solid rgb(142, 5, 194);
  color: rgb(62, 6, 95);
}
.project .actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
h3 {
  cursor: pointer;
}
.actions .icons .material-icons {
  color: rgb(0, 0, 0, 0.3);
  cursor: pointer;
  margin-right: 5px;
}
.actions .icons .material-icons:hover {
  color: rgb(0, 0, 0, 0.6);
}
.project.complete {
    border-left: 4px solid rgb(78, 159, 61);
}
.project.complete .tick {
    color: rgb(78, 159, 61);
}
.project.complete .tick:hover {
  color: rgb(78, 159, 61);
}
</style>