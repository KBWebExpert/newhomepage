<script setup>
import { forceReflow } from '@/utils/_';
import { ref } from 'vue';

const containerEl = ref()
const contentEl = ref()
const MainMenuEl = ref()


const onBeforeEnter = (el) => {
    containerEl.value.dataset.type = 'spilt'
    el.style.transitionDuration = '0ms'
    el.style.setProperty('--tw-rotate-y', '180deg')
    forceReflow()
    el.style.transitionDuration = ''
}

const onEnter = (el, done) => {
    let _event = null;
    MainMenuEl.value.addEventListener('transitionend', _event = (ev) => {
        if (ev.target != MainMenuEl.value || ev.propertyName !== 'transform') return;
        ev.stopPropagation()
        MainMenuEl.value.removeEventListener('transitionend', _event)

        contentEl.value.style.setProperty('--tw-rotate-y', '180deg')
        let __event = null;

        let _time = Date.now()
        contentEl.value.addEventListener('transitionend', __event = (ev) => {
            if (ev.target != contentEl.value || ev.propertyName !== 'transform') return;
            ev.stopPropagation()
            if (Date.now() - _time < 500) {
                return
            }
            contentEl.value.removeEventListener('transitionend', __event)
            done()
        })
    })
}

const onLeave = (el, done) => {
    setTimeout(() => {
        done()
    }, 900)
}

const onAfterEnter = (el) => {
    containerEl.value.dataset.type = ''
    let _event = null;
    MainMenuEl.value.addEventListener('transitionend', _event = (ev) => {
        if (ev.target != MainMenuEl.value || ev.propertyName !== 'transform') return;
        ev.stopPropagation()
        MainMenuEl.value.removeEventListener('transitionend', _event)


        el.style.transitionDuration = '0ms'
        contentEl.value.style.transitionDuration = '0ms'
        el.style.transitionDelay = '0ms'
        contentEl.value.style.transitionDelay = '0ms'

        el.style.setProperty('--tw-rotate-y', '')
        contentEl.value.style.setProperty('--tw-rotate-y', '')
        forceReflow()
        el.style.transitionDuration = ''
        contentEl.value.style.transitionDuration = ''
        el.style.transitionDelay = ''
        contentEl.value.style.transitionDelay = ''
    })
}

const onAfterLeave = (el) => {
    el.style.transitionDuration = '0ms'
    el.style.opacity = 0
    forceReflow()
    el.style.transitionDuration = ''
}
</script>

<template>
    <div class="transition-page-wrapper">
        <div ref="containerEl" class="index-page-container">
            <div ref="MainMenuEl" class="main-menu-wrapper">
                <RouterLink to="/" class="menu-item">
                    <i-icon-park-outline-bear />
                    我
                </RouterLink>
                <RouterLink to="/friends" class="menu-item">
                    <i-icon-park-outline-notebook />
                    朋友们
                </RouterLink>
                <RouterLink to="/projects" class="menu-item">
                    <i-icon-park-outline-experiment-one />
                    实验室
                </RouterLink>
            </div>
            <div ref="contentEl" class="content-wrapper">
                <RouterView v-slot="{ Component }">
                    <Transition @before-enter="onBeforeEnter" @enter="onEnter" @leave="onLeave"
                        @after-enter="onAfterEnter" @after-leave="onAfterLeave" :css="false">
                        <component :is="Component" />
                    </Transition>
                </RouterView>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.transition-page-wrapper {
    @apply "w-[350px] sm:w-[400px] h-[500px]";
    @apply "absolute top-1/2 left-1/2 rounded-4xl";
    @apply "-translate-x-1/2 -translate-y-1/2";


    .index-page-container {
        @apply "transform-gpu translate-z-200vh h-full";
        @apply "shadow-2xl shadow-primary/30";
        @apply "rounded-4xl";
        @apply "bg-white";
        @apply flex flex-col;
        @apply transition-colors delay-300 duration-0;

        .main-menu-wrapper {
            @apply "rounded-t-4xl";
            @apply "bg-white";
            @apply "flex justify-center space-x-3";
            @apply "py-6";
            @apply "select-none";
            transition-property: transform, border-radius;
            transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
            transition-duration: 300ms;

            .menu-item {
                @apply relative;
                @apply text-primary/70 font-semibold text-lg;
                @apply cursor-pointer z-0;
                @apply transition-all duration-300;
                @apply flex items-center;

                svg {
                    @apply text-0em;
                    @apply mr-0;
                    @apply transition-all;
                }

                &::before {
                    content: "";
                    @apply absolute bottom-0 left-0 right-0 h-3px -z-1;
                    @apply rounded-xl;
                    @apply bg-primary-extralight;
                    @apply transition-all;
                }

                &:hover,
                &.router-link-exact-active {
                    @apply px-3;

                    &::before {
                        @apply h-full;
                    }

                    svg {
                        @apply text-0.9em mr-1;
                    }
                }

                &:hover {
                    svg {
                        animation: backshake 0.5s linear;
                    }
                }

                &.router-link-exact-active {
                    @apply pointer-events-none;
                }
            }
        }

        .content-wrapper {
            @apply "flex-grow relative";
            @apply "bg-white";
            @apply "rounded-b-4xl transform-gpu translate-z-200vh";
            transition: border-radius cubic-bezier(0.4, 0, 0.2, 1) 300ms, transform cubic-bezier(0.4, 0, 0.2, 1) 1000ms;
            transform-style: preserve-3d;
        }
    }

    .index-page-container[data-type="spilt"] {
        @apply shadow-none;
        @apply pointer-events-none;
        @apply bg-transparent;
        @apply transition-none;

        .main-menu-wrapper {
            @apply rounded-4xl;
            @apply "shadow-2xl shadow-primary/30";
            @apply transform-gpu -translate-y-4;
        }

        .content-wrapper {
            @apply rounded-4xl;
            @apply "shadow-2xl shadow-primary/30";
        }
    }
}
</style>