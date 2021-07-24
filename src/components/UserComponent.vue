<template>
    <div>
        <div>{{ label }} : {{ user.name }}</div>
        <button @click="counterAge">counter age</button>
        <div>{{ showAge }}</div>
        
        <hr>
        <slot name="description"></slot>
    </div>
</template>

<script>
import { computed, toRef, toRefs } from '@vue/runtime-core';
export default {
    props: {
        label: {
            type: String,
            default: 'label'
        },
        user: {
            type: Object,
            default: () => {}
        }
    },
    setup(props, { emit, attrs, slots }) {
        console.log(emit);
        console.log(attrs);
        console.log(slots);
        const { label, user } = toRefs(props);

        const showAge = computed({
            get: () => {
                return user.value.age;
            }
        });

        const counterAge = () => {
            emit('counterAge', user.value.age + 1);
        }

        return {
            showAge,
            counterAge
        }
    }
}
</script>