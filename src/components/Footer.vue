<template>
    <footer>
        @ {{ new Date().getFullYear() }} <a href="https://daidr.me" target="_blank">DAIDR'S HOUSE</a>
    </footer>
</template>

<style scoped lang="scss">
footer {
    @apply opacity-50 select-none whitespace-nowrap;
    @apply fixed -z-1;
    @apply bottom-5 left-1/2;
    @apply transform-gpu -translate-x-1/2;
    @apply "text-white text-3xl md:text-5xl";
    font-family: BEYNO-online !important;

    a {
        font-family: BEYNO-online !important;
        @apply relative;

        &::after {
            content: "";
            @apply "absolute bottom-10px left-0 w-0 h-2px md:h-4px";
            @apply rounded-full;
            @apply bg-white;
            @apply transition-all duration-400;
        }

        &:hover::after {
            @apply w-full;
        }
    }
}
</style>