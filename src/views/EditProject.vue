<template>
  <div class="edit">
    <h2>Edit Project</h2>
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label>Title :</label>
        <input
          type="text"
          v-model="title"
          :class="{ 'has-error': titleBool }"
          @keyup="validationTitle"
        />
        <div class="invalid-feedback">
          <p>{{ titleError }}</p>
        </div>
      </div>
      <div class="form-group">
        <label>Detail :</label>
        <textarea
          v-model="detail"
          :class="{ 'has-error': detailBool }"
          @keyup="validationDetail"
        ></textarea>
        <div class="invalid-feedback">
          <p>{{ detailError }}</p>
        </div>
      </div>
      <button type="submit" class="submit">Update</button>
      <button
        class="back"
        @click="
          {
            this.$router.push({ name: 'Home' });
          }
        "
      >
        Back
      </button>
    </form>
  </div>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      project: null,
      title: "",
      detail: "",
      titleError: "",
      titleBool: false,
      detailError: "",
      detailBool: false,
      url: "http://localhost:3000/projects/" + this.id,
    };
  },
  methods: {
    handleSubmit() {
      let editProject = {
        title: this.title,
        details: this.detail,
      };

      if (this.title === "") {
        this.titleError = "Please enter title";
        this.titleBool = true;
      } else if (this.title.length <= 5) {
        this.titleError = "Title must be at least 5 chars long";
        this.titleBool = true;
      }

      if (this.detail === "") {
        this.detailError = "Please enter detail";
        this.detailBool = true;
      } else if (this.detail.length <= 10) {
        this.detailError = "Detail must be at least 10 chars long";
        this.detailBool = true;
      }

      if (!this.titleBool && !this.detailBool) {
        fetch(this.url, {
          method: "PATCH",
          headers: { "Content-type": "application/json" },
          body: JSON.stringify(editProject),
        })
          .then(() => this.$router.push({ name: "Home" }))
          .catch((err) => console.log(err.message));
      }
    },
    validationTitle() {
      if (this.title === "") {
        this.titleError = "Please enter title";
        this.titleBool = true;
        return true;
      } else if (this.title.length <= 5) {
        this.titleError = "Title must be at least 5 chars long";
        this.titleBool = true;
        return true;
      }
      this.titleError = "";
      this.titleBool = false;
      return false;
    },
    validationDetail() {
      if (this.detail === "") {
        this.detailError = "Please enter detail";
        this.detailBool = true;
        return true;
      } else if (this.detail.length <= 10) {
        this.detailError = "Detail must be at least 10 chars long";
        this.detailBool = true;
        return true;
      }
      this.detailError = "";
      this.detailBool = false;
      return false;
    },
  },
  mounted() {
    fetch(this.url)
      .then((res) => res.json())
      .then((data) => {
        (this.title = data.title), (this.detail = data.details);
      })
      .catch((err) => console.log(err.message));
  },
};
</script>

<style>
.edit {
  max-width: 500px;
  margin: 30px auto;
  background: #fff;
  text-align: left;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 0 10px #ccc;
}
.edit h2 {
  text-align: center;
  font-size: 1.6em;
  font-weight: bold;
}
.edit label {
  color: #bbb;
  display: block;
  margin: 20px 0 10px 0;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
.edit input {
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  border: 0;
  border-bottom: 1px solid #ddd;
}

.edit textarea {
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  border: 1px solid #ddd;
  height: 100px;
}

.edit input:focus,
textarea:focus {
  outline: none !important;
  border: 1px solid #ddd;
  box-shadow: 0 0 10px #719ece;
}

.edit button {
  display: inline-block;
  font-size: 16px;
  border: 0;
  padding: 10px;
  margin: 20px auto 0;
  border-radius: 6px;
  cursor: pointer;
}

.edit button.submit {
  background: #00ce89;
  color: #fff;
  margin-right: 10px;
}
.edit button.back {
  background: #ccc;
  color: #fff;
}
.edit input.has-error,
.edit select.has-error {
  border: 1px solid #eb1c38 !important;
}
.edit label.has-error {
  color: #eb1c38;
}
.invalid-feedback p {
  color: #eb1c38;
  font-weight: bold;
  font-size: 0.8em;
  margin-top: 5px;
}
</style>
