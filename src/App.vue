<template>
  <div class="container py-5">
    <h1 class="text-center mb-4">Моя коллекция книг</h1>

    <!-- Форма добавления -->
    <div class="row justify-content-center mb-4">
      <div class="col-lg-8">
        <form @submit.prevent="addBook" class="card p-4 shadow-sm">
          <h4 class="mb-3">Добавить книгу</h4>

          <div class="mb-3">
            <label class="form-label">Название</label>
            <input
              v-model="newBook.title"
              type="text"
              class="form-control"
              placeholder="Введите название"
              required
            />
          </div>

          <div class="mb-3">
            <label class="form-label">Описание</label>
            <textarea
              v-model="newBook.description"
              class="form-control"
              rows="3"
              placeholder="Введите описание"
              required
            ></textarea>
          </div>

          <div class="mb-3">
            <label class="form-label">Ссылка на фото</label>
            <input
              v-model="newBook.image"
              type="url"
              class="form-control"
              placeholder="https://..."
              required
            />
          </div>

          <button class="btn btn-primary" type="submit">
            Добавить
          </button>
        </form>
      </div>
    </div>

    <!-- Панель сортировки -->
    <div class="row mb-4">
      <div class="col-lg-8 mx-auto">
        <div class="d-flex flex-wrap gap-2 align-items-center justify-content-between">
          <h4 class="mb-0">Список книг</h4>

          <select v-model="sortOrder" class="form-select w-auto">
            <option value="asc">Сортировка: А–Я</option>
            <option value="desc">Сортировка: Я–А</option>
          </select>
        </div>
      </div>
    </div>

    <!-- Карточки -->
    <div class="row">
      <div
        v-for="book in sortedBooks"
        :key="book.id"
        class="col-md-6 col-lg-4 mb-4"
      >
        <div class="card h-100 shadow-sm">
          <img
            :src="book.image"
            class="card-img-top"
            alt="Обложка книги"
            style="height: 250px; object-fit: cover;"
          />

          <div class="card-body d-flex flex-column">
            <h5 class="card-title">{{ book.title }}</h5>
            <p class="card-text flex-grow-1">
              {{ book.description }}
            </p>

            <button
              @click="removeBook(book.id)"
              class="btn btn-danger btn-sm mt-3"
            >
              Удалить
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Если список пуст -->
    <div v-if="sortedBooks.length === 0" class="text-center text-muted mt-4">
      Пока нет добавленных книг
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const books = ref([
  {
    id: 1,
    title: 'Гарри Поттер',
    description: 'Серия фэнтези-романов о юном волшебнике и его приключениях.',
    image: 'https://unsplash.com/photos/stack-of-books-on-white-table-YuUz5uQkaXM'
  },
  {
    id: 2,
    title: '1984',
    description: 'Антиутопия Джорджа Оруэлла о тотальном контроле и свободе.',
    image: 'https://unsplash.com/photos/a-person-holding-up-a-book-in-front-of-a-book-shelf-kUYexCmEPuI'
  }
])

const newBook = ref({
  title: '',
  description: '',
  image: ''
})

const sortOrder = ref('asc')

const addBook = () => {
  books.value.push({
    id: Date.now(),
    title: newBook.value.title,
    description: newBook.value.description,
    image: newBook.value.image
  })

  newBook.value = {
    title: '',
    description: '',
    image: ''
  }
}

const removeBook = (id) => {
  books.value = books.value.filter(book => book.id !== id)
}

const sortedBooks = computed(() => {
  return [...books.value].sort((a, b) => {
    if (sortOrder.value === 'asc') {
      return a.title.localeCompare(b.title, 'ru')
    }
    return b.title.localeCompare(a.title, 'ru')
  })
})
</script>