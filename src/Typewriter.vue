<template>
    <div class="typewriter" :style="textStyles">
        {{ displayedText }}<span :class="['cursor', {'cursor--static': isTyping }]" :style="finalCursorStyles"></span>
    </div>
</template>

<script setup>
import {ref, onMounted, watch, onUnmounted, computed} from 'vue';

const {speed, delay, textStyles, cursorStyles, loop} = defineProps({
    speed: {
        type: Number,
        default: 400
    },
    delay: {
        type: Number,
        default: 5000
    },
    textStyles: {
        type: Object,
        default: () => ({})
    },
    cursorStyles: {
        type: Object,
        default: () => ({})
    },
    loop: {
        type: Boolean,
        default: true
    },
});

const defaultCursorStyles = {
    width: '12px',
    height: '1em',
    backgroundColor: 'currentColor'
};

// Computed property to merge cursor styles

const finalCursorStyles = computed(() => {
    return {
        ...defaultCursorStyles,
        ...cursorStyles
    };
});

// No need to pass any arguments to defineSlots() for the default slot
const slots = defineSlots();

// Safely extract content from the default slot
const slotContent = slots.default ? slots.default() : [];
const text = slotContent.length > 0 ? slotContent[0].children : '';

const displayedText = ref("");
let index = 0;
const isTyping = ref(true);
let typeInterval;

onMounted(() => { startTyping(text) });

const startTyping = (typingText) => {

    //console.log('startTyping called with:', typingText);

    if (!typingText || !typingText.length) return;

    // Begin with the cursor blinking
    isTyping.value = false;

    setTimeout(() => {
        index = 0

        function startDeletion(){
            // Start deletion process
            isTyping.value = true;
            typeInterval = setInterval(() => {
                if (displayedText.value.length > 0) {
                    displayedText.value = displayedText.value.slice(0, -1);
                } else {
                    clearInterval(typeInterval);
                    isTyping.value = false; // Typing (and deletion) is done
                    if (loop) {
                        startTyping(typingText); // Restart the entire process if loop is true
                    }
                }
            }, speed);
        }

        typeInterval = setInterval(() => {
            if (index < typingText.length) {
                displayedText.value += typingText.charAt(index);
                index++;
                //console.log('Current displayedText:', displayedText.value);
            } else {
                clearInterval(typeInterval);
                isTyping.value = false;
                if (loop) {
                    // Start the delay after typing finishes only if loop is true
                    setTimeout(() => { startDeletion(); }, delay);
                }
            }
        }, speed);
    }, delay);
};

onUnmounted(() => { clearInterval(typeInterval); });

</script>

<style scoped>

.typewriter {
    /* Default styles for the typewriter wrapper.
       If you had any default styles specifically for the text,
       they would go under .typewriter or another relevant class. */
}

.cursor {
    display: inline-block;
    width: 12px;
    height: 1em;
    background-color: currentColor;
    animation: blink 1s steps(2, start) infinite;
}

@keyframes blink {
    to {
        visibility: hidden;
    }
}

.cursor--static {
    animation: none;
}
</style>
