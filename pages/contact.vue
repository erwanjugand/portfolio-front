<template>
  <main>
    <section class="container">
      <h1 :background="$t('contact.mainTitle')" v-text="$t('contact.mainTitle')" />
      <PCard class="form-contact">
        <template #header>
          <h2>
            <PIcon name="envelopeOpenText" />
            {{ $t('contact.title') }}
          </h2>
        </template>
        <PForm :data="form" secure @submit="sendEmail">
          <PField
            v-model="form.email"
            required
            type="email"
            :label="$t('contact.email')"
            autocomplete="email"
          />
          <PField
            v-model="form.title"
            type="text"
            required
            :label="$t('contact.titleMessage')"
          />
          <PField
            v-model="form.message"
            required
            type="textarea"
            :min-rows="2"
            :label="$t('contact.message')"
          />
          <PTransitionFadeHeight tag="ul">
            <div v-if="emailStatus === 'sent' || emailStatus === 'error'">
              <div :class="emailStatus === 'sent' ? 'message-success' : 'message-fail'">
                <PIcon :name="emailStatus === 'sent' ? 'checkCircle' : 'timesCircle'" />
                {{ $t(`contact.validation.${emailStatus}`) }}
              </div>
            </div>
          </PTransitionFadeHeight>
          <PButton icon="sparkles" :loading="loading">
            {{ $t('contact.send') }}
          </PButton>
        </PForm>
      </PCard>
    </section>
    <PFooter />
  </main>
</template>

<script lang="ts">
import Vue from 'vue'
import { MetaInfo } from 'vue-meta'

export default Vue.extend({

  data () {
    return {
      form: {
        email: '',
        title: '',
        message: ''
      },
      emailStatus: null as 'sent' | 'error' | null,
      loading: false
    }
  },

  head (): MetaInfo {
    return {
      title: this.$t('contact.metaTitle').toString()
    }
  },

  nuxtI18n: {
    paths: {
      fr: '/contact',
      en: '/contact'
    }
  },

  methods: {
    async sendEmail () {
      this.form = {
        email: '',
        title: '',
        message: ''
      }

      this.loading = true
      // TODO : POST email datas
      await new Promise(resolve => setTimeout(resolve, 5000))
      this.loading = false
      this.emailStatus = 'sent'
      setTimeout(() => { this.emailStatus = null }, 5000)
    }
  }
})
</script>

<style lang="scss">
.form-contact {
  max-width: 640px;
  margin: auto;
}
</style>
