<script lang="ts" setup>
import { ref } from 'vue';
import { useAppStore } from '../store';
import { useI18n } from 'vue-i18n';
import { RiTranslate, RiEnglishInput } from '@remixicon/vue';

const appStore = useAppStore();
const language = ref(appStore.language);
const { t, locale } = useI18n();

const toggleLanguage = () => {
  const newLanguage = language.value === 'en' ? 'zh' : 'en';
  appStore.updateLanguage(newLanguage);
  language.value = newLanguage;
  locale.value = newLanguage;
  document.title = t('title');
};
</script>

<script lang="ts">
export default {
  name: 'LanguageSelect'
};
</script>

<template>
  <div class="language-toggle">
    <t-button @click="toggleLanguage" variant="outline" shape="circle" theme="primary">
      <template #icon>
        <RiTranslate v-if="language === 'zh'" size="14" />
        <RiEnglishInput v-else size="14" />
      </template>
    </t-button>
  </div>
</template>

<style scoped>
.language-toggle {
  /* 样式根据需要进行修改 */
}
</style>