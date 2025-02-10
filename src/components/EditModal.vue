<template>
    <div class="fixed inset-0 flex items-center justify-center bg-gray-400/50 z-1">
      <div class="bg-white dark:bg-slate-700 p-4 rounded shadow m-4 lg:m-0">
        <h3 class="mb-2 text-lg font-bold text-black dark:text-white">Edit Value</h3>
        <h4 class="text-black dark:text-white">You are now editing the timeseries value of <strong>{{ currentCountry }}</strong> for the date: <strong>{{ currentDate }}</strong>.</h4>
        <span class="text-sm text-black dark:text-white"><em>Note: The new value should be between <strong>-2000</strong> and <strong>2000</strong>.</em></span>
        <form>
            <label for="newValue" class="text-sm text-black dark:text-white">New value:</label>
            <input
                type="number"
                min="-2000"
                max="2000"
                id="newValue"
                v-model="newValue"
                class="border p-1 mb-4 w-full text-black dark:text-white bg-slate-200 dark:bg-slate-800"
                />

            <span v-if="isInvalidInput" class="text-red-400">Warning: New value should be between -2000 and 2000!</span>
            <div class="flex justify-end gap-2">
                <button class="px-3 py-1 disabled:bg-green-300 bg-green-500 text-white rounded cursor-pointer disabled:cursor-not-allowed" :disabled="isInvalidInput" @click="saveEdit(newValue)">Save</button>
                <button class="px-3 py-1 bg-gray-500 text-white rounded cursor-pointer" @click="closeModal">Cancel</button>
            </div> 
        </form>
      </div>
    </div>
</template>

<script>
import { computed } from 'vue';
import { ref } from 'vue';

    export default {
        props: {
            currentValue: Number,
            currentCountry: String,
            currentDate: String
        },
        methods: {
            closeModal() {
                event.preventDefault()
                this.$emit('modalClosed', null)
            },
            saveEdit(newValue) {
                event.preventDefault()
                this.$emit('modalClosed', newValue)
            }
        },
        setup(props) {
            const newValue = ref(props.currentValue);

            // Computed value for valid input (-2000 <= X <= 2000, including 0)
            const isInvalidInput = computed(() => newValue.value < -2000 || newValue.value > 2000 || (!newValue.value && newValue.value !== 0))

            return {newValue, isInvalidInput}
        }
    }
</script>