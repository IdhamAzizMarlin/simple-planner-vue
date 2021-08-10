<template>
  <div class="container">
    <div class="content">
      <FilterNav @filterChange="current = $event" :current="current" />
      <div v-if="projects.length">
        <div v-for="project in filteredProject" :key="project.id">
          <SingleProject
            :project="project"
            @delete="handleDelete"
            @complete="handleComplete"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";
export default {
  name: "Home",
  components: {
    SingleProject,
    FilterNav,
  },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id;
      });
    },
    handleComplete(id) {
      let p = this.projects.find((project) => {
        return project.id === id;
      });
      p.complete = !p.complete;
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  computed: {
    filteredProject() {
      return this.projects.filter((project) => {
        if (this.current === "all") {
          return project;
        } else if (this.current === "completed") {
          return project.complete === true;
        } else {
          return project.complete === false;
        }
      });
    },
  },
};
</script>
