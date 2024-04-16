<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
    title: String,
    author: String,
    year: Number,
    id: String,
    fetchBooks: Function as () => () => Promise<void> // Define fetchBooks as a function returning Promise<void>
})

const deleteBook = async (id: string) => {
    try {
        // Check if ID is valid
        if (!id) {
            console.error('Invalid book ID')
            return
        }

        // Send DELETE request to delete the book
        const response = await fetch(`http://localhost:5000/book/${id}`, {
            method: 'DELETE'
        })

        if (response.ok) {
            // Call fetchBooks function after successful deletion
            await (props.fetchBooks as () => Promise<void>)() // Assert and await fetchBooks
        } else {
            // Handle unsuccessful DELETE request
            console.error('Failed to delete book')
        }
    } catch (error) {
        // Handle network errors or other exceptions
        console.error('Error deleting book:', error)
    }
}
</script>

<template>
    <div :key="props.id">
        <h4>{{ props.title }}</h4>
        <h4>{{ props.id }}</h4>
        <h4>{{ props.author }}</h4>
        <h4>{{ props.year }}</h4>
        <button @click="deleteBook(props.id)">Delete Book</button>
    </div>
</template>

<style scoped></style>
