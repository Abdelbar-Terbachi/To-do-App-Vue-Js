<template>
  <div class="project" :class="{completed: project.completed }">
    <div class="actions">
      <h3 @click="showDetails">{{project.title}}</h3>
      <div class="icons">
        <router-link :to="{name:'EditProject',params:{id:project.id}}">
          <i class="fas fa-pen"></i>
        </router-link>
        <i class="fas fa-trash" @click="deleteItem"></i>
        <i class="fas fa-check tick" @click="toggleComplete"></i>
      </div>
    </div>
    <div class="details" v-if="show">
      <p>{{project.details}}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      show: false,
      uri: "http://localhost:3000/projects/" + this.project.id
    };
  },
  methods: {
    showDetails() {
      this.show = !this.show;
    },
    deleteItem() {
      fetch(this.uri, { method: "delete" })
        .then(this.$emit("delete", this.project.id))
        .catch(err => console.log(err.message));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ completed: !this.project.completed })
      })
        .then(() => this.$emit("complete", this.project.id))
        .catch(err => console.log(err.message));
    }
  }
};
</script>

<style>
.project {
  width: 50%;
  margin: 20px auto;
  background-color: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
  border-left: 4px solid #e90074;
}
.project h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.fas {
  padding: 10px 15px;
  cursor: pointer;
  color: rgb(197, 196, 194);
}
.fas:hover {
  color: rgb(102, 100, 100);
}
.project.completed {
  border-left: 4px solid #00ce89;
}
.project.completed .tick {
  color: #00ce89;
}
</style>