<script setup>
import { ref, onMounted, computed, watch } from 'vue'
// ref --> state management
// onMount --> rendering page
// computed --> A to B or A to C (date will be used in the filtering the list)
// watch --> change of the reference

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() =>
	todos.value.sort((a, b) => {
		return b.createdAt - a.createdAt
	})
)

const addTodo = () => {
	if (input_content.value.trim() === '' || input_category.value === null) {
		return
	}

	todos.value.push({
		id: Math.random(),
		content: input_content.value,
		category: input_category.value,
		done: false,
		createdAt: new Date().getTime()
	})

}

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

onMounted(() => {
	name.value = localStorage.getItem('name')
})
</script>

<template>
	<main class="class">
		<section class="greeting">
			<h2 class="title">
				Whats up, <input type="text" placeholder="Name Here" v-model="name" />
			</h2>
		</section>
		<section class="create-todo">
			<h3>CREATE A TODO</h3>
			<form @submit.prevent="addTodo"></form>
			<h4>What's on your todo list today?</h4>
			<input type="text" placeholder="e.g make" v-model="input_content" />

			<h4>Pick a category</h4>
			<div class="options">
				<label>
					<input type="radio" name="category" id="category1" value="business" v-model="input_category" />
					<span class="bubble business"></span>
					<div>Business</div>
				</label>

				<label>
					<input type="radio" name="category" value="personal" v-model="input_category" />
					<span class="bubble personal"></span>
					<div>Personal</div>
				</label>
			</div>

			<input type="submit" value="Add todo" />
		</section>
	</main>
</template>
