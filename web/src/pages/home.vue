<template>
  <div id="home" class="flex size-full items-center justify-center overflow-y-auto h-screen ">
    <div class="mt-20 flex w-full flex-col gap-4 p-4 sm:mt-10 lg:max-w-3xl xl:max-w-4xl">
      <div class="flex items-center justify-center gap-2">
        <img src="https://io.onenov.cn/file/202405120135970.svg" class="w-64" />
      </div>
      <div class="flex items-center justify-center gap-2">
        <span class="text-3xl font-bold dark:text-gray-100">InstCopilot AI Search</span>
      </div>
      <div class="flex items-center justify-center gap-2">
        <span class="text-xl mb-4 font-bold dark:text-gray-100">{{ t('subtitle') }}</span>
      </div>
      <SearchInputBar :autofocus="true" :loading="false" @search="search" />
      <div class="flex flex-wrap gap-4 justify-center items-center my-2">
        <SearchMode />
        <SearCategory v-if="enableAdvanced" />
      </div>
      <div class="fixed top-0 left-0 flex items-center justify-end gap-2 p-4">
        <div class="w-32">
          <ModelSelect />
        </div>
        <div class="w-28">
          <SearchEngineSelect />
        </div>
      </div>
      <div class="fixed top-0 right-0 flex items-center justify-center gap-2 p-4">
        <t-switch class="w-12" size="large" :default-value="appStore.theme === 'dark'" @change="onChangeTheme">
          <template #label="slotProps">
            <template v-if="slotProps.value">
              <RiMoonLine size="14" />
            </template>
            <template v-else>
              <RiSunLine size="14" />
            </template>
          </template>
        </t-switch>
        <div>
          <LanguageSelect />
        </div>
      </div>
      <div class="w-full flex justify-center">
        <div class="flex flex-wrap justify-center gap-2">
          <t-tag v-for="(item, index) in list" :key="index" shape="round" variant="light" size="medium"
            class="cursor-pointer p-4 hover:opacity-80" @click="onQuickSearch(item)">
            {{ item }}
            <RiSearch2Line class="ml-1" size="12" />
          </t-tag>
        </div>
      </div>
      <div class="mt-4">
        <PageFooter />
      </div>
    </div>
  </div>
</template>

<script setup lang="tsx">
import router from '../router';
import { RiSearch2Line, RiSunLine, RiMoonLine } from '@remixicon/vue';
import { PageFooter, SearchInputBar, SearCategory, SearchMode, ModelSelect, LanguageSelect,SearchEngineSelect } from '../components';
import { useI18n } from 'vue-i18n';
import { computed } from 'vue';
import { useAppStore } from '../store';
import { type SwitchValue } from 'tdesign-vue-next';
const onChangeTheme = (val: SwitchValue) => {
  if (val) appStore.updateTheme('dark');
  else appStore.updateTheme('light');
};

const { t } = useI18n();
const { locale } = useI18n();
const appStore = useAppStore();

const enableAdvanced = computed(() => appStore.engine === 'SEARXNG')

const quickly: Record<string, string[]> = {
  zh: [
    '什么是大语言模型LLM?',
    '怎么使用Ollama在本地部署大模型?',
    '苹果开发者大会？',
    '编程实现最长公共子序列？',
  ],
  en: [
    'What is LLM?',
    'What is RAG?',
    'How to use LLM in enterprise?'
  ]
};

const list = computed(() => {
  const key = locale.value;
  return quickly[key];
});

const search = (val: string) => {
  if (!val) {
    return;
  }
  router.push({
    name: 'SearchPage',
    query: {
      q: val
    }
  });
};

const onQuickSearch = (val: string) => {
  search(val);
};
</script>

<script lang="tsx">
export default {
  name: 'HomePage'
};
</script>
