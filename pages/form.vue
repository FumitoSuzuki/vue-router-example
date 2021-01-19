<template>
  <div class="container">
    <div>
      <header>
        <h1>Form <span class="name">Example</span></h1>
      </header>
      <section class="body">
        <NuxtChild
          v-bind="{ formName, form, values }"
          @update="formUpdate"
          @reset="formReset"
          keep-alive
        />
      </section>
      <p><NLink to="/" class="button--grey">Back home</NLink></p>
      <footer>
        <small>Form Example by Starter for CodeSandBox</small>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const form = await $content("form/define").fetch();
    return {
      form: {
        input: form.input,
        select: form.select,
      },
    };
  },
  data() {
    return {
      formName: "contact",
      values: { input: 'abc', select: 'A' },
    };
  },
  methods: {
    formUpdate(e) {
      this.values = e;
    },
    formReset(e) {
      Object.assign(this.$data, this.$options.data.call(this));
    },
  },
};
</script>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
h1 {
  font-weight: 400;
}
.name {
  color: #00C48D;
}
p,
.body {
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>
