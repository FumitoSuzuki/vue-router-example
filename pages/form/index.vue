<template>
  <div class="child">
    <p>This is Form Index Component.</p>
    <form
      ref="observer"
      :name="formName"
      @submit.prevent="toConfirm"
      @reset.prevent="toReset"
    >
      <div class="form-element">
        <label for="form.input.id">{{ form.input.label }}</label>
        <input type="text" id="form.input.id" v-model="commit.input" />
      </div>
      <div class="form-element">
        <label for="form.select.id">{{ form.select.label }}</label>
        <select
          id="form.select.id"
          name="form.select.id"
          v-model="commit.select"
        >
          <option
            v-for="(item, key) in form.select.option"
            :key="key"
            :value="item.value"
          >
            {{ item.text }}
          </option>
        </select>
      </div>
      <div class="form-element flex">
        <button type="reset">Reset</button>
        <button type="submit">Confirm</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    form: { type: Object, default: () => {} },
    values: { type: Object, default: () => {} },
    formName: { type: String, default: 'default' },
  },
  computed: {
    commit() {
      return this.values;
    },
  },
  watch: {
    commit: {
      handler() {
        this.$emit("update", this.commit);
      },
      deep: true,
    },
  },
  methods: {
    toConfirm() {
      this.$router.push("/form/confirm");
    },
    toReset() {
      this.$emit('reset', true)
      this.$refs.observer.reset();
    },
  },
};
</script>

<style scoped>
p {
  margin-bottom: 10px;
}
label {
  display: block;
}
.form-element {
  text-align: left;
  margin-bottom: 10px;
}
.form-element:last-child {
  margin-bottom: 0;
}
.form-element.flex {
  display: flex;
  justify-content: space-between;
}
label {
  margin-bottom: 0.3rem;
}
input,
select {
  width: 100%;
}
button {
  width: 48%;
  margin-top: 10px;
}
</style>
