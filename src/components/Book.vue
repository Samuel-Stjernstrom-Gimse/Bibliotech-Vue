<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
    title: String,
    author: String,
    year: Number,
    id: Number,
    user: Object,
    book: Object,
    fetchBooks: {
        type: Function as () => Promise<void>,
        required: true
    }
})

const rentBook = async (id: number, userId: number) => {
    try {
        const bookingDate = new Date().toISOString() // Get current date/time in ISO format
        const returnDate = new Date().toISOString()
        const newRent = {
            booking_id: 120,
            user_id: userId,
            user: props.user,
            book_id: id,
            book: props.book,
            booking_date: bookingDate, // This will send a timestamp in milliseconds
            return_date: returnDate // This will send the current date and time
        }

        console.log(newRent)

        const response = await fetch(`http://localhost:5000/booking/${id}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(newRent)
        })

        if (response.ok) {
            console.log('success')
        } else {
            console.log('Failed to rent the book:', response.statusText)
        }
    } catch (error) {
        console.log('Error renting the book:', error)
    }
}

const deleteBook = async (id: number) => {
    try {
        if (!id) {
            console.error('Invalid book ID')
            return
        }

        const response = await fetch(`http://localhost:5000/book/${id}`, {
            method: 'DELETE'
        })

        if (response.ok) {
            await (props.fetchBooks as () => Promise<void>)()
        } else {
            console.error('Failed to delete book')
        }
    } catch (error) {
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
        <button @click="deleteBook(props.id!)">Delete Book</button>
        <button @click="rentBook(props.id!, props.userId!)">Rent Book</button>
    </div>
</template>

<style scoped></style>
