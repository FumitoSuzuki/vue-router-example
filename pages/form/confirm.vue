<template>
  <div>
    <p>This is Form Confirm Component.</p>
    <div class="result">
      <p v-for="(item, key) in mapped" :key="key">
        <span>{{ key }}:</span>{{ item }}
      </p>
    </div>
    <div class="form-element flex">
      <button @click="toBack">Back</button>
      <button @click="sendData">Commit</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    formName: { type: String, default: "form" },
    form: { type: Object, default: () => {} },
    values: { type: Object, default: () => {} },
  },
  computed: {
    mapped() {
      const mapped = {};
      for (const [key, value] of Object.entries(this.form)) {
        let propVal = this.values[key];
        if ("option" in value) {
          const result = this.getOptValue(propVal, value.option);
          propVal = Boolean(result) ? result.text : "";
        }
        mapped[value.label] = propVal;
      }
      return mapped;
    },
  },
  methods: {
    getOptValue(value, array) {
      return array.find((item) => item.value === value);
    },
    encode(data) {
      return Object.keys(data)
        .map((key) => {
          if (typeof data[key] === "object") return this.encode(data[key]);
          return `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`;
        })
        .join("&");
    },
    sendData() {
      const url = "/";
      const encode = this.encode({
        "form-name": this.formName,
        ...this.mapped,
      });
      const config = {
        header: { "Content-Type": "application/x-www-form-urlencoded" },
      };
      this.$axios
        .$post(url, encode, config)
        .then(() => {
          this.$router.push(`/form/result/?${encode}`);
        })
        .catch((error) => {
          if (this.$axios.isCancel(error)) {
            console.log("Request canceled", error);
          } else {
            console.log(error);
          }
        });
    },
    toBack() {
      this.$router.push(`/form/`);
    },
  },
};
</script>

<style scoped>
p {
  margin-bottom: 10px;
}
.result p {
  text-align: left;
}
.result p span {
  display: inline-block;
  width: 9rem;
  text-align: right;
  margin-right: 1rem;
}
.form-element.flex {
  display: flex;
  justify-content: space-between;
}
button {
  width: 48%;
}
</style>
