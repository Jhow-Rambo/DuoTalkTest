<script lang="ts" setup>
import { useChatStore } from '@/views/apps/chat/useChatStore';
import { avatarText } from '@core/utils/formatters';
import { PerfectScrollbar } from 'vue3-perfect-scrollbar';
import { useChat } from './useChat';

const emit = defineEmits<{
  (e: 'close'): void
}>()

const store = useChatStore()

const { resolveAvatarBadgeVariant } = useChat()

// eslint-disable-next-line prefer-const
let tags: { id: number; name: string; value: string }[] = [
  { id: 0, name: 'Lorem Ipsum', value: 'Lorem Ipsum' },
]

let nameTag = ''

let valueTag = ''

const addNewTag = () => {
  (nameTag && valueTag) && tags.push({ id: tags.length, name: nameTag, value: valueTag })
  console.log(tags)
  nameTag = ''
  valueTag = ''
}

const deleteTag = (id: number) => {
  tags.splice(id, 1)
}
</script>

<template>
  <template v-if="store.activeChat">
    <!-- Close Button -->
    <div
      class="pt-2 me-2"
      :class="$vuetify.locale.isRtl ? 'text-left' : 'text-right'"
    >
      <VBtn
        variant="text"
        color="default"
        icon
        size="small"
        @click="$emit('close')"
      >
        <VIcon
          size="24"
          icon="tabler-x"
          class="text-medium-emphasis"
        />
      </VBtn>
    </div>

    <!-- User Avatar + Name + Role -->
    <div class="text-center px-6">
      <VBadge
        location="bottom right"
        offset-x="7"
        offset-y="4"
        bordered
        :color="resolveAvatarBadgeVariant(store.activeChat.contact.status)"
        class="chat-user-profile-badge mb-5"
      >
        <VAvatar
          size="80"
          class="bg-surface"
          variant="tonal"
          :class="`text-${resolveAvatarBadgeVariant(store.activeChat.contact.status)}`"
        >
          <VImg
            v-if="store.activeChat.contact.avatar"
            :src="store.activeChat.contact.avatar"
          />
          <span
            v-else
            class="text-3xl"
          >{{ avatarText(store.activeChat.contact.fullName) }}</span>
        </VAvatar>
      </VBadge>
      <h2 class="mb-1 font-weight-medium text-high-emphasis text-base">
        {{ store.activeChat.contact.fullName }}
      </h2>
      <p class="text-capitalize text-sm text-medium-emphasis">
        {{ store.activeChat.contact.role }}
      </p>
    </div>

    <!-- User Data -->
    <PerfectScrollbar
      class="ps-chat-user-profile-sidebar-content text-medium-emphasis pb-5 px-5"
      :options="{ wheelPropagation: false }"
    >
      <!-- About -->
      <div class="my-8">
        <span
          for="textarea-user-about"
          class="text-sm text-disabled"
        >ABOUT</span>
        <p class="mt-2">
          {{ store.activeChat.contact.about }}
        </p>
      </div>

      <!-- TAGS -->
      <div class="mt-8">
        <span
          for="textarea-user-about"
          class="text-sm text-disabled"
        >TAGS</span>
        <!-- TODO: Add tags -->
        <div
          v-for="tag in tags"
          :key="tag.id"
        >
          <div class="d-flex ">
            <p class="px-2">
              {{ tag.name }}:
            </p>
            <p>{{ tag.value }}</p>
            <p
              class="px-1"
              @click="deleteTag(tag.id)"
            >
              x
            </p>
          </div>
        </div>
      </div>

      <!-- Add Tags -->
      <div class="mb-8">
        <span
          for="textarea-user-about"
          class="text-sm text-disabled"
        >Adicionar Tag</span>
        <div class="d-flex">
          <VTextField
            v-model="nameTag"
            :maxlength="20"
            label="Nome"
            autofocus
          />
          <VTextField
            v-model="valueTag"
            :maxlength="20"
            autofocus
            label="Valor"
          />
          <VBtn @click="addNewTag">
            +
          </VBtn>
        </div>
      </div>
    </PerfectScrollbar>
  </template>
</template>
