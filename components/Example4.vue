<script>
export default {
  name: 'Example4',
  data() {
    return {
      form: {
        name: ''
      },
      response: null,
      error: null
    }
  },
  methods: {
    async randomCat(name) {
      try {
        const res = await this.$axios.$get(
          `/.netlify/functions/random-cat?name=${name}`
        )
        this.response = res
        this.error = null
      } catch (e) {
        this.error = e.response
        this.response = null
      }
    }
  }
}
</script>

<template>
  <ElForm
    ref="form"
    :model="form"
    @submit.native.prevent="randomCat(form.name)"
    inline
    label-width="auto"
    label-position="left"
  >
    <h2>4. Get a random cat with your name</h2>
    <p><em>API call done by your browser</em></p>
    <ElFormItem label="Name">
      <ElInput
        v-model="form.name"
        data-cy="input-random-cat"
        placeholder="Your name"
        required
      />
    </ElFormItem>
    <ElButton
      @click="randomCat(form.name)"
      type="primary"
      data-cy="btn-random-cat"
    >
      🐈 Meow
    </ElButton>
    <p>
      Response:
      <br />
      <br />
      <img
        v-show="response"
        :src="response"
        data-cy="img-random-cat"
        style="width:100%;height:auto;"
      />
    </p>
    <p v-if="error" style="color:red;">
      <strong>Error {{ error.status }}</strong>
      <br />
      {{ error.data }}
    </p>
  </ElForm>
</template>
