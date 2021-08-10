<template>
  <div class="add">
    <h2>Add Project</h2>
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
      <button type="submit" class="submit">Add</button>
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
  data() {
    return {
      title: "",
      detail: "",
      url: "http://localhost:3000/projects/",
      titleError: "",
      titleBool: false,
      detailError: "",
      detailBool: false,
    };
  },
  methods: {
    handleSubmit() {
      let project = {
        title: this.title,
        details: this.detail,
        complete: false,
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
          method: "POST",
          headers: { "Content-type": "application/json" },
          body: JSON.stringify(project),
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
};
</script>

<style>
.add {
  max-width: 500px;
  margin: 30px auto;
  background: #fff;
  text-align: left;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 0 10px #ccc;
}
.add h2 {
  text-align: center;
  font-size: 1.6em;
  font-weight: bold;
}
.add label {
  color: #bbb;
  display: block;
  margin: 20px 0 10px 0;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
.add input {
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  border: 0;
  border-bottom: 1px solid #ddd;
}

.add textarea {
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  border: 1px solid #ddd;
  height: 100px;
}

.add input:focus,
textarea:focus {
  outline: none !important;
  border: 1px solid #ddd;
  box-shadow: 0 0 10px #719ece;
}

.add button {
  display: inline-block;
  font-size: 16px;
  border: 0;
  padding: 10px;
  margin: 20px auto 0;
  border-radius: 6px;
  cursor: pointer;
}

.add button.submit {
  background: #00ce89;
  color: #fff;
  margin-right: 10px;
}
.add button.back {
  background: #ccc;
  color: #fff;
}
.add input.has-error,
.add select.has-error {
  border: 1px solid #eb1c38 !important;
}
.add label.has-error {
  color: #eb1c38;
}
.invalid-feedback p {
  color: #eb1c38;
  font-weight: bold;
  font-size: 0.8em;
  margin-top: 5px;
}
</style>
