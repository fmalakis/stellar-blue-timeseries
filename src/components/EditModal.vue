<template>
    <div class="fixed inset-0 flex items-center justify-center bg-gray-500/50 z-1">
      <div class="bg-white p-4 rounded shadow">
        <h3 class="mb-2 text-lg font-bold">Edit Value</h3>
        <h4>You are now editing the timeseries value of <strong>{{ currentCountry }}</strong> for the date: <strong>{{ currentDate }}</strong>.</h4>
        <form>
            <label for="newValue">New value:</label>
            <input
                type="number"
                min="-2000"
                max="2000"
                id="newValue"
                v-model="newValue"
                class="border p-1 mb-4 w-full"
                />
                <h1>{{ typeofnewValue }}</h1>
                <div class="flex justify-end gap-2">
                <button class="px-3 py-1 disabled:bg-green-300 bg-green-500 text-white rounded cursor-pointer disabled:cursor-not-allowed" :disabled="(newValue < -2000 || newValue > 2000) && newValue" @click="saveEdit(newValue)">Save</button>
                <button class="px-3 py-1 bg-gray-500 text-white rounded cursor-pointer" @click="closeModal">Cancel</button>
            </div> 
        </form>
      </div>
    </div>
</template>

<script>
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

            return {newValue}
        }
    }
</script>