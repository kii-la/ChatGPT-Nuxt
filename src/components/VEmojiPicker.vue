<script lang="ts" setup>
import { ref, computed } from "vue"

type EmojiCategory = {
  title: string
  icon: string
  emojis: string[]
}

const props = defineProps({
  modelValue: {
    type: String,
    required: true,
  },
})
const emit = defineEmits(["onSelected"])

const searchQuery = ref("")
const recentEmojis = ref<string[]>([])
const selectedCategoryIndex = ref(0)

const emojiCategories: EmojiCategory[] = [
  {
    icon: "mdi:recent",
    title: "Recently",
    emojis: ["🐶", "🐱", "🐭", "🐹", "🐰", "🐻", "🐼", "🐨"],
  },
  {
    icon: "mdi:emoticon-outline",
    title: "Smileys & Emotion",
    emojis: ["😀", "😃", "😄", "😁", "😆", "😅", "😂", "🤣", "😊", "😉", "😍", "😘", "😜", "😝", "😋", "😛"],
  },
  {
    icon: "mdi:dog",
    title: "Animals & Nature",
    emojis: [
      "🐶",
      "🐱",
      "🦁",
      "🐯",
      "🐴",
      "🐭",
      "🐹",
      "🦊",
      "🐻",
      "🐼",
      "🐨",
      "🐮",
      "🐷",
      "🐗",
      "🐔",
      "🐣",
      "🐸",
      "🐟",
      "🐠",
      "🐳",
      "🐬",
      "🐊",
      "🐢",
      "🐍",
      "🦕",
      "🦖",
      "🦜",
    ],
  },
  {
    icon: "mdi:hamburger",
    title: "Food & Drink",
    emojis: [
      "🍔",
      "🍟",
      "🍕",
      "🌭",
      "🥪",
      "🍣",
      "🍱",
      "🍛",
      "🍝",
      "🍜",
      "🍲",
      "🍔",
      "🍺",
      "🍻",
      "🍷",
      "🥤",
      "🧊",
      "🍩",
      "🍰",
      "🎂",
      "🍪",
      "🍫",
      "🍬",
      "🍭",
    ],
  },
  {
    icon: "mdi:football",
    title: "Activities",
    emojis: ["⚽️", "🏀", "🏈", "⚾️", "🎾", "🏐", "🏉", "🥊", "🏋️‍♀️", "🤸", "🚴‍♀️", "🤹", "🎮", "🎲"],
  },
  {
    icon: "mdi:email-outline",
    title: "Objects",
    emojis: [
      "☎️",
      "💻",
      "🖥",
      "🖨",
      "📱",
      "🎧",
      "🎤",
      "📷",
      "📹",
      "💡",
      "🔍",
      "🔐",
      "🚪",
      "💳",
      "💵",
      "🏺",
      "🔑",
      "🧸",
      "🎁",
    ],
  },
  {
    icon: "mdi:earth",
    title: "Travel & Places",
    emojis: [
      "🌍",
      "🌎",
      "🌏",
      "🌋",
      "🏜️",
      "🏕️",
      "🏞️",
      "🌅",
      "🌄",
      "🏰",
      "🌉",
      "🎡",
      "🎢",
      "🏟️",
      "🚂",
      "🛵",
      "🛴",
      "🏍️",
      "🚲",
      "🛬",
      "🚀",
    ],
  },
  {
    icon: "mdi:lightning-bolt-outline",
    title: "Symbols",
    emojis: [
      "❤️",
      "💔",
      "💭",
      "💬",
      "🔥",
      "🌟",
      "⭐️",
      "🌞",
      "🌚",
      "🌀",
      "🌈",
      "💡",
      "✨",
      "🎉",
      "🎊",
      "🎁",
      "🔨",
      "💣",
      "🚽",
      "🚪",
    ],
  },
  {
    icon: "mdi:flag-variant-outline",
    title: "Flags",
    emojis: [
      "🇨🇳",
      "🇺🇸",
      "🇬🇧",
      "🇯🇵",
      "🇰🇷",
      "🇿🇦",
      "🇪🇸",
      "🇫🇷",
      "🇩🇪",
      "🇮🇳",
      "🇲🇾",
      "🇳🇪",
      "🇵🇹",
      "🇷🇺",
      "🇸🇦",
      "🇸🇬",
      "🇹🇷",
      "🇻🇳",
      "🏴󠁧󠁢󠁥󠁮󠁧󠁿",
    ],
  },
]

const currentCategoryEmojis = computed(() => {
  return emojiCategories[selectedCategoryIndex.value].emojis.filter((emoji) => emoji.includes(searchQuery.value))
})

function selectCategory(index: number) {
  selectedCategoryIndex.value = index
}

function selectEmoji(emoji: string) {
  if (!recentEmojis.value.includes(emoji)) {
    recentEmojis.value.unshift(emoji)
    if (recentEmojis.value.length > 10) {
      recentEmojis.value.pop()
    }
  }
  emit("onSelected", emoji)
}
</script>
<template>
  <div class="shadow-xs w-[320px] rounded-md bg-white" ref="elementRef">
    <div class="grid grid-cols-9 gap-1">
      <div
        v-for="(category, index) in emojiCategories"
        :key="index"
        class="flex h-[30px] w-[30px] cursor-pointer items-center justify-center rounded hover:bg-gray-200"
        @click="selectCategory(index)"
        :class="{ 'bg-gray-300': selectedCategoryIndex === index }"
      >
        <Icon color="gray" :name="category.icon" size="2em">
          {{ category.title }}
        </Icon>
      </div>
    </div>

    <div class="mt-1 grid grid-cols-9 gap-1">
      <div
        class="flex h-[30px] w-[30px] cursor-pointer items-center justify-center rounded hover:bg-gray-200"
        v-for="emoji in currentCategoryEmojis"
        :key="emoji"
      >
        <Icon
          :name="emoji"
          size="2em"
          style="font-size: 2em; line-height: 1em; width: 1em; height: 1em"
          @click="selectEmoji(emoji)"
        />
      </div>
    </div>
  </div>
</template>
