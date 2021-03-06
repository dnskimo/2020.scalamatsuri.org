<i18n>
## language=yaml
  en:
    lang: "Talking in"
    contribute: "OSS work"
    keywords: "Keywords"
    tag: "Categories"
    speaker_experience: "Speaker Experience"
  ja:
    lang: "発表言語"
    contribute: "OSS 活動"
    keywords: "キーワード"
    tag: "カテゴリ"
    speaker_experience: "スピーカー経験"
</i18n>

<template>
  <div ref="modalKeyListener" class="modal_inner" tabindex="0" @keyup.escape="$emit('close')">
    <h2 class="modal_title">
      <a :href="`proposals/${program.id}`" target="_blank">
        {{ program[$i18n.locale].title }}
      </a>
    </h2>
    <ul class="modal_speakers">
      <li v-for="speaker in program[$i18n.locale].speakers" :key="speaker.id" class="modal_speaker">
        <div class="modal_speaker_icon" :style="`backgroundImage: url('${speaker.icon}')`" />
        <p class="modal_speaker_name">
          {{ speaker.name }}
        </p>
        <p class="modal_speaker_org">
          {{ speaker.organization }}
        </p>
        <p class="modal_speaker_id">
          <a v-if="speaker.twitter" class="modal_speaker_sns" :href="`https://twitter.com/${speaker.twitter}`">
            <img v-lazy="require('~/assets/img/common/icon-sns-tw.svg')">
            {{ speaker.twitter }}
          </a>
          <a v-if="speaker.github" class="modal_speaker_sns" :href="`https://github.com/${speaker.github}`">
            <img v-lazy="require('~/assets/img/common/icon-sns-git.svg')">{{ speaker.github }}
          </a>
          <!-- TODO: show other_sns field -->
        </p>
      </li>
    </ul>
    <div class="modal_text">
      <p v-text="program[$i18n.locale].detail" />
    </div>
    <div class="modal_scopeArea">
      <dl class="modal_scope">
        <dt>
          {{ $t('lang') }}
        </dt>
        <dd>
          {{ program[$i18n.locale].language }}
        </dd>
      </dl>
      <dl class="modal_scope">
        <dt>
          {{ $t('keywords') }}
        </dt>
        <dd>
          <ul>
            <li v-for="kw in program[$i18n.locale].keywords" :key="kw">
              {{ kw }}
            </li>
          </ul>
        </dd>
      </dl>
      <dl class="modal_scope">
        <dt>
          {{ $t('tag') }}
        </dt>
        <dd>
          <ul>
            <li v-for="tag in program[$i18n.locale].tags" :key="tag">
              {{ tag }}
            </li>
          </ul>
        </dd>
      </dl>
      <div v-for="speaker in program[$i18n.locale].speakers" :key="speaker.name">
        <dl v-if="speaker.contributes.length > 0" class="modal_scope_large">
          <dt v-if="program[$i18n.locale].speakers.length === 1">
            {{ $t('contribute') }}
          </dt>
          <dt v-else>
            {{ speaker.name }} <br> {{ $t('contribute') }}
          </dt>
          <dd>
            <p v-for="contribute in speaker.contributes" :key="contribute">
              {{ contribute }}
            </p>
          </dd>
        </dl>
        <dl v-if="speaker.speaker_experience.length > 0" class="modal_scope_large">
          <dt v-if="program[$i18n.locale].speakers.length === 1">
            {{ $t('speaker_experience') }}
          </dt>
          <dt v-else>
            {{ speaker.name }} <br> {{ $t('speaker_experience') }}
          </dt>
          <dd>
            <p v-for="ex in speaker.speaker_experience" :key="ex">
              {{ ex }}
            </p>
          </dd>
        </dl>
      </div>
    </div>
    <div class="modal_close" @click="$emit('close')">
      閉じる
    </div>
  </div>
</template>

<script>
export default {
  props: {
    program: {
      type: Object,
      required: true
    }
  },
  mounted: function () {
    // For closing modal by pushing ESC key.
    this.$nextTick(this.$refs.modalKeyListener.focus())
  }
}
</script>
