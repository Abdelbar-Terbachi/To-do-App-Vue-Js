<template>
  <form @submit.prevent="handleSubmit">
    <label>Title:</label>
    <input required type="text" v-model="title" />
    <label>Details:</label>
    <textarea required v-model="details"></textarea>
    <button>Add Project</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      details: ""
    };
  },
  methods: {
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.details,
        completed: false
      };
      fetch("http://localhost:3000/projects", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project)
      })
        .then(() => {
          this.$router.push("/");
        })
        .catch(err => console.log(err.message));
    }
  }
};
</script>

<style>
form {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  width: 50%;
  margin: auto;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #dddddd;
  width: 100%;
  box-sizing: border-box;
  outline: none;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
  outline: none;
}
form button {
  display: block;
  margin: 20px auto 0px;
  background-color: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
}
</style>
