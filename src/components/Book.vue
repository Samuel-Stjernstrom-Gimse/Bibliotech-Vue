<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
    title: String,
    author: String,
    year: Number,
    id: Number,
    fetchBooks: {
        type: Function as () => Promise<void>,
        required: true
    }
})

const rentBook = async (id: number, userId: number) => {
    try {
        if (!id) {
            console.error(' 404 book not found: ', id)
            return
        }

        const newRent = {
            booking_id: 0,
            user_id: userId,
            book_id: id,
            booking_date: Date.now(),
            return_date: new Date()
        }

        const response = await fetch(`http://localhost:5000/books/rent${id}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(newRent)
        })

        if (response.ok) {
            console.log('success')
        }
    } catch (error) {
        console.log('error')
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
        <button>Rent Book</button>
    </div>
</template>

<style scoped></style>
