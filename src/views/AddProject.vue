<template>
  <div class="add-project">
    <form @submit.prevent="handleSubmit">
      <label for="project-title">Title:</label>
      <input
        type="text"
        id="project-title"
        v-model.trim.lazy="title"
        required
      />

      <label for="project-details">Details<small> (Optional)</small>:</label>
      <textarea id="project-details" v-model.trim.lazy="details"></textarea>

      <button>Add</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "AddProject",

  data() {
    return {
      title: "",
      details: "",
    };
  },

  methods: {
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.details,
        completed: false,
      };

      // fetch("http://localhost:3000/projects", {
      fetch("https://plan-things-default-rtdb.firebaseio.com/projects.json", {
        method: "POST",
        // header is saying we are sending data of json type
        headers: { "Content-Type": "application/json" },
        // the data we're sending
        body: JSON.stringify(project),
      })
        .then(() => {
          this.$router.push("/");
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
form {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
}

label {
  display: block;
  color: #bbb;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}

input {
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid #ddd;
  width: 100%;
  outline: none;
}

textarea {
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid #ddd;
  width: 100%;
  height: 100%;
  resize: vertical;
  outline: none;
}

form button {
  display: block;
  margin: 20px auto 0;
  background-color: #25d483;
  color: #fff;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  outline: none;
}
</style>
