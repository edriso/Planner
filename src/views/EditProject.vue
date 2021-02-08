<template>
  <div class="edit-project">
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

      <button>Update</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "EditProject",

  props: ["id"],

  data() {
    return {
      title: "",
      details: "",
      // uri: "http://localhost:3000/projects/" + this.id,
      uri:
        "https://plan-things-default-rtdb.firebaseio.com/projects/" +
        this.id +
        ".json",
    };
  },

  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          title: this.title,
          details: this.details,
        }),
      })
        .then(this.$router.push("/"))
        .catch((err) => console.log(err));
    },
  },

  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      })
      .catch((err) => console.log(err.message));
  },
};
</script>
