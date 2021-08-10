<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <div class="project-title" @click="toggleDetail">
        <p>{{ project.title }}</p>
      </div>
      <div class="icons">
        <span
          class="material-icons"
          @click="
            {
              this.$router.push({
                name: 'EditProject',
                params: { id: project.id },
              });
            }
          "
          >edit</span
        >
        <span class="material-icons" @click="deleteProject">delete</span>
        <span
          class="material-icons"
          @click="completeProject"
          :class="{ tick: project.complete }"
          >done</span
        >
      </div>
    </div>
    <div class="details" v-if="showDetail">
      <p>
        {{ project.details }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetail: false,
      url: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    toggleDetail() {
      this.showDetail = !this.showDetail;
    },
    deleteProject() {
      fetch(this.url, { method: "DELETE" })
        .then(() => this.$emit("delete", this.project.id))
        .catch((err) => console.log(err.message));
    },
    completeProject() {
      fetch(this.url, {
        method: "PATCH",
        headers: { "Content-type": "application/json" },
        body: JSON.stringify({ complete: !this.project.complete }),
      })
        .then(() => this.$emit("complete", this.project.id))
        .catch((err) => console.log(err.message));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
.complete {
  border-left: 4px solid green;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.actions p {
  font-size: 1em;
  font-weight: bold;
  cursor: pointer;
}
.complete .actions p {
  font-weight: normal;
}
.actions .material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}

.actions .material-icons:hover {
  color: #777;
}
.complete .actions .icons .tick {
  color: green;
}
</style>
