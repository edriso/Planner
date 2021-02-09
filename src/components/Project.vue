<template>
  <div class="project" :class="{ completed: project.completed }">
    <div class="actions">
      <h3 @click="toggleDetails">
        {{ project.title }}
      </h3>

      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons">edit</span>
        </router-link>
        <span class="material-icons" @click="deleteProject">delete</span>
        <span class="material-icons done" @click="toggleComplete">done</span>
      </div>
    </div>
    <div class="details" v-show="showDetails" v-if="project.details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Project",

  props: {
    project: Object,
  },

  data() {
    return {
      showDetails: false,
      // uri: "http://localhost:3000/projects/" + this.project.id,
      uri:
        "https://plan-things-default-rtdb.firebaseio.com/projects/" +
        this.project.id +
        ".json",
    };
  },

  methods: {
    toggleDetails() {
      if (this.project.details) {
        this.showDetails = !this.showDetails;
      }
    },

    deleteProject() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.log(err));
    },

    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ completed: !this.project.completed }),
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((err) => console.log(err));
    },
  },

  // for firebase database
  // mounted() {
  //   fetch(this.uri, {
  //     method: "PATCH",
  //     headers: { "Content-Type": "application/json" },
  //     body: JSON.stringify({ id: 0 }),
  //   })
  //     .then(() => {
  //       this.$emit("complete", this.project.id);
  //     })
  //     .catch((err) => console.log(err));
  // },
};
</script>

<style>
.project {
  margin: 1.25rem auto;
  background-color: #fff;
  padding: 0.625rem 1.25rem;
  border-radius: 0.25rem;
  box-shadow: 0.0625rem 0.125rem 0.1875rem rgba(0, 0, 0, 0.05);
  border-left: 0.25rem solid #f9483e;
  transition: border-color 150ms ease-in-out;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.actions h3 {
  cursor: pointer;
}

.actions .icons span {
  font-size: 18px;
  cursor: pointer;
  color: #bbb;
  user-select: none;
  transition: color 150ms ease-in-out;
}

.actions .icons span:hover {
  color: #777;
}

.actions .icons span:nth-child(2) {
  margin: 0 0.5rem;
}

.project.completed {
  border-left-color: #25d483;
}

.project.completed .icons .done {
  color: #25d483;
}
</style>
