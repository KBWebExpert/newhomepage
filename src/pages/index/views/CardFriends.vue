<script setup>
import LazyImage from '@/components/LazyImage.vue';
import { FriendsList } from '@/config/friends.js';
import { shuffle } from '@/utils/_';
import { ref } from 'vue';
import CardWrapper from './CardWrapper.vue';

// 打乱 FriendsList
const FriendsListShuffled = shuffle(FriendsList);

const showTopShadow = ref(false);
const showBottomShadow = ref(true);

const onScroll = (e) => {
    const { scrollTop, scrollHeight, clientHeight } = e.target;
    showTopShadow.value = scrollTop > 3;
    showBottomShadow.value = scrollHeight - scrollTop > clientHeight;
};

</script>

<template>
    <CardWrapper>
        <div class="friends-overflow-container">
            <Transition name="fade">
                <div v-if="showTopShadow" class="top-shadow"></div>
            </Transition>
            <Transition name="fade">
                <div v-if="showBottomShadow" class="bottom-shadow"></div>
            </Transition>
            <div class="friends-container y-scroll-box" @scroll.passive="onScroll">
                <a v-for="item of FriendsListShuffled" :key="item.url" class="friend-item" :href="item.url"
                    target="_blank" rel="noopener noreferrer">
                    <div class="avatar" :style="{ backgroundColor: item.color }">
                        <LazyImage :src="`/images/friends/${item.avatar}`" :alt="item.name" />
                    </div>
                    <div class="detail">
                        <div class="name">
                            {{ item.name }}
                        </div>
                        <div class="motto">
                            {{ item.motto }}
                        </div>
                    </div>
                </a>
            </div>
        </div>
    </CardWrapper>

</template>

<style scoped lang="scss">
.friends-overflow-container {
    @apply h-full overflow-hidden rounded-b-4xl relative;
    @apply py-15px px-2;

    .top-shadow {
        @apply "absolute top-14px left-3 right-5 h-6 z-10";
        @apply "bg-gradient-to-t from-transparent to-white";
        @apply "pointer-events-none";
    }

    .bottom-shadow {
        @apply "absolute bottom-14px left-3 right-5 h-6 z-10";
        @apply "bg-gradient-to-b from-transparent to-white";
        @apply "pointer-events-none";
    }

    .friends-container {
        @apply h-full;
        @apply "grid grid-cols-1 gap-3";
        @apply px-3 py-5px;
        scrollbar-width: thin;
        scrollbar-color: rgb(var(--color-primary) / 0.8) rgb(var(--color-primary-extralight));

        // 滚动条
        &::-webkit-scrollbar {
            @apply w-2;
        }

        &::-webkit-scrollbar-thumb {
            @apply bg-primary/80;
            @apply rounded-full;
        }

        &::-webkit-scrollbar-track {
            @apply bg-primary-extralight;
            @apply rounded-full;
        }

        .friend-item {
            @apply flex;
            @apply bg-primary-extralight;
            @apply rounded-2xl p-2;
            @apply cursor-pointer relative;
            @apply transition-all;

            &::before {
                content: "";
                @apply absolute -top-6px -left-6px -right-6px -bottom-6px -z-1;
                @apply rounded-1.4rem;
                @apply border-4px border-primary-extralight;
                @apply transition;
                @apply transform-gpu scale-50 opacity-0;
                @apply pointer-events-none;
            }

            &:hover::before,
            &:active::before {
                @apply scale-100 opacity-100;
            }

            .avatar {
                @apply w-13 h-13 rounded-xl mr-2 select-none;
                @apply flex-shrink-0;

                .lazy-image {
                    @apply rounded-xl;

                    &:deep(img) {
                        @apply rounded-xl;
                    }
                }
            }

            .detail {
                @apply text-primary;
                @apply flex flex-col;

                .name {
                    @apply text-lg font-bold;
                }

                .motto {
                    @apply text-base text-primary-medium;
                }
            }
        }
    }


}
</style>