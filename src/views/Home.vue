<template>
  <!-- $event === the parameter "by" -->
  <FilterNav :current="current" @filterChange="current = $event" />

  <div class="home">
    <div v-if="projects.length">
      <Project
        :project="project"
        v-for="project in filteredProjects"
        :key="project.id"
        @delete="handelDelete"
        @complete="handleComplete"
      />
    </div>
  </div>
</template>

<script>
import Project from "@/components/Project";
import FilterNav from "@/components/FilterNav.vue";

export default {
  name: "Home",

  components: { Project, FilterNav },

  data() {
    return {
      projects: [],
      current: "all",
    };
  },

  computed: {
    filteredProjects() {
      if (this.current === "completed") {
        return this.projects.filter((project) => project.completed);
      }

      if (this.current === "ongoing") {
        return this.projects.filter((project) => !project.completed);
      }

      return this.projects;
    },
  },

  methods: {
    handelDelete(projectId) {
      this.projects = this.projects.filter((project) => {
        return project.id !== projectId;
      });
    },
    handleComplete(projectId) {
      let p = this.projects.find((project) => {
        return project.id === projectId;
      });
      p.completed = !p.completed;
    },
  },

  mounted() {
    // fetch("http://localhost:3000/projects")
    fetch("https://plan-things-default-rtdb.firebaseio.com/projects.json")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
};
</script>
