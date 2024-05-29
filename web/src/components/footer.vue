<script lang="ts" setup>
import { onMounted, ref } from 'vue';
import { useAppStore } from '../store';
import { useI18n } from 'vue-i18n';

type Emit = {
  (e: 'click'): void;
};

const appStore = useAppStore();
const { t } = useI18n();
const model = ref('');

const emits = defineEmits<Emit>();

onMounted(() => {
  const cached = appStore.model;
  model.value = cached || '';
});
</script>

<script lang="ts">
export default {
  name: 'PageFooter',
};
</script>

<template>
  <div class="flex w-full flex-col items-center gap-2">
    <div class="mt-2 flex items-center gap-2 text-xs text-black dark:text-white">
      <div class="cursor-pointer" @click="emits('click')">{{ t('selectModel') }}:
        <span v-if="!appStore.enableLocal" shape="round" size="small">{{ appStore.model || t('message.noSelect')
          }}</span>
        <span v-else shape="round" size="small">{{ t('localModel') }}: {{ appStore.localModel || t('message.noSelect')
          }}</span>
      </div>
      <div class="cursor-pointer" @click="emits('click')">
        {{ t('search') }}: <span shape="round" size="small">{{ appStore.engine || t('message.noSelect') }}</span>
      </div>
    </div>
    <div class="text-center text-xs text-gray-400 mb-2">
      {{ t('warning') }}
    </div>
    <a href="https://orence.net" target="bank">
      <t-button size="small" shape="round" variant="text">
        潮汐助手-更好的AI与增强的答案
      </t-button>
    </a>
  </div>
</template>
