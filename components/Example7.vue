<script>
export default {
  name: 'Example7',
  data() {
    return {
      form: {
        name: ''
      },
      response: '—',
      error: null
    }
  },
  methods: {
    async sendSlackMessage(name) {
      try {
        const res = await this.$axios.$post(
          '/.netlify/functions/slack-webhook',
          { name }
        )
        this.response = res
        this.error = null
      } catch (e) {
        this.error = e.response
        this.response = '—'
      }
    }
  }
}
</script>

<template>
  <ElForm
    ref="form"
    :model="form"
    @submit.native.prevent="sendSlackMessage(form.name)"
    inline
    label-width="auto"
    label-position="left"
  >
    <h2>7. Send a Slack message</h2>
    <ElFormItem label="Name">
      <ElInput v-model="form.name" placeholder="Your name" />
    </ElFormItem>
    <ElButton @click="sendSlackMessage(form.name)" type="primary">
      👋 Say hello
    </ElButton>
    <p>Response: {{ response }}</p>
    <p v-if="error" style="color:red;">
      <strong>Error {{ error.status }}</strong>
      <br />
      {{ error.data }}
    </p>
  </ElForm>
</template>
