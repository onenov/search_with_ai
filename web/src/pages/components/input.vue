<script setup lang="ts">
import { ref } from 'vue';
import { useI18n } from 'vue-i18n';
import { RiArrowUpLine } from '@remixicon/vue';
type Emits = {
  (e: 'ask', val: string): void
}
type Props = {
  loading: boolean
}
const { t } = useI18n();
const props = defineProps<Props>();

const emits = defineEmits<Emits>();

const query = ref('');

const onAsk = () => {
  if (!query.value.trim()) {
    query.value = '';
    return;
  }
  emits('ask', query.value);
  query.value = '';
};
</script>
<script lang="ts">
export default {
  name: 'ChatInput'
};
</script>

<template>
  <div id="ask" class="flex items-center justify-center">
    <div
      class="w-10/12 overflow-hidden rounded-3xl border border-gray-100 p-1 bg-white dark:bg-transparent dark:bg-zinc-700 flex items-center justify-center gap-2">
      <t-input v-model="query" :disabled="props.loading" clearable :autofocus="false" :maxlength="100" size="large"
        :placeholder="t('tips.continue')" @enter="onAsk">
      </t-input>
      <div class="w-10">
        <t-button class="right-buttom" :disabled="loading" shape="square" variant="base" @click="onAsk">
        <template #icon>
          <RiArrowUpLine />
        </template>
      </t-button>
      </div>
    </div>
  </div>
</template>


<style scoped>
#ask {
  --td-radius-default: 25rem;
}

.right-buttom {
  margin-right: -8px;
}
</style>
