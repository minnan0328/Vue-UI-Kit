<template>
    <div class="owl-carousel" ref="owlCarousels"
        :style="{ '--customItemWidth': customItemWidth ? `${customItemWidth}px` : `100vw`,
                  '--customItemHeight': customItemHeight ? `${customItemHeight}px` : `100vh`,
                  '--perPage': perPage,
                  '--currentSlide': currentSlide
                }">

        <div class="carousel-inner" ref="carouselsInner"
            @mousedown.prevent="handleMouseDown"
            @mouseleave.prevent="handleMouseLeave"
            @mouseup.prevent="handleMouseUp"
            @mousemove.prevent="handleMouseMove"
            @touchstart.prevent="handleMouseDown"
            @touchcancel.prevent="handleMouseLeave"
            @touchend.prevent="handleMouseUp"
            @touchmove.prevent="handleMouseMove"
            :style="{ '--carouselLeft': carouselState.carouselLeft }">
            <slot :currentSlide="currentSlide"></slot>
        </div>

        <div :class="['navigation', { 'nav-small': navigationBtnSmall }]" v-if="navigationEnabled">
            <button class="btn-icon btn-navigation icon-arrow-left" @click.self="prevSlide()"
                    @mouseover="pauseAutoPlay" @mouseout="enabledAutoPlay"
                    @touchstart="pauseAutoPlay" @touchend="enabledAutoPlay">
            </button>
            <button class="btn-icon btn-navigation icon-arrow-right" @click.self="nextSlide()"
                    @mouseover="pauseAutoPlay" @mouseout="enabledAutoPlay"
                    @touchstart="pauseAutoPlay" @touchend="enabledAutoPlay">
            </button>
        </div>

        <div class="pagination" v-if="paginationEnabled">
            <button :class="['btn-icon btn-pagination', { 'active': currentSlide == idx }]"
                    v-for="(slideBtn, idx) of slideAmount" :key="idx" @click.self="changeSlide(idx)"
                    @mouseover="pauseAutoPlay" @mouseout="enabledAutoPlay"
                    @touchstart="pauseAutoPlay" @touchend="enabledAutoPlay">
            </button>
        </div>
    </div>
</template>
<script>

import { ref, computed, onMounted } from 'vue';
import { useSlide } from '@/components/owl-carousel/hooks/sliding-hook.js';

export default {
    name: 'owl-carousel',
    props: {
        perPage: {
            type: Number,
            default: 1
        },
        /* ???????????? */
        autoPlay: {
            type: Boolean,
            default: true
        },
        loops: {
            type: Boolean,
            default: false
        },
        /* ????????????????????????????????? */
        timeDetention: {
            type: Number,
            default: 5000
        },
        /* ??????????????????????????? */
        navigationEnabled: {
            type: Boolean,
            default: true
        },
        /* ??????????????????????????? */
        paginationEnabled: {
            type: Boolean,
            default: true
        },
        /* ????????????????????????????????? */
        navigationBtnSmall: {
            type: Boolean,
            default: false
        },
        customItemWidth: {
            type: Number,
            default: null
        },
        customItemHeight: {
            type: Number,
            default: null
        }
    },
    setup(props) {

        const owlCarousels = ref(null);
        const carouselsInner = ref(null);

        /* ???????????????????????????????????????????????????????????????????????? */
        const navigationEnabled = computed(() => props.navigationEnabled ? props.navigationEnabled : true);
        /* ???????????????????????????????????????????????????????????????????????? */
        const paginationEnabled = computed(() => props.paginationEnabled ? props.paginationEnabled : true);

        const {
            currentSlide, slideAmount, isTransitionend, carouselState,
            prevSlide, nextSlide, changeSlide, enabledAutoPlay, pauseAutoPlay, toggleActive,
            handleMouseDown, handleMouseMove ,handleMouseLeave, handleMouseUp
        } = useSlide({props, carouselsInner});

        const tD  = () => {
            console.log('tD');
        };
        const tE  = () => {
            console.log('tE');
        };
        const tC  = () => {
            console.log('tC');
        };
        const tM  = () => {
            console.log('tM');

        };

        onMounted(() => {
            carouselsInner.value.ontransitionend = () => toggleActive();
            carouselsInner.value.ontransitioncancel = () => toggleActive();
        });

        return {
            owlCarousels, carouselsInner, carouselState, currentSlide, slideAmount, isTransitionend,
            navigationEnabled, paginationEnabled,
            handleMouseDown, handleMouseLeave, handleMouseUp, handleMouseMove,
            prevSlide, nextSlide, changeSlide, enabledAutoPlay, pauseAutoPlay,
            tD, tE, tC, tM
        };
    }
}
</script>
<style lang="scss" scoped src="./owl-carousel.scss"></style>