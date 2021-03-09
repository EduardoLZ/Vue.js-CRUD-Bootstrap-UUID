<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <h2>Salvando el semestre - Youtube Channel</h2>
      </div>
    </div>
    <div class="row">
      <div class="col-4">
        <form>
          <div class="mb-3">
            <label class="form-label">Book</label>
            <input v-model="book" type="text" class="form-control" />
          </div>
          <button
            v-if="!updateState"
            @click="addBook"
            type="button"
            class="btn btn-primary"
          >
            Add Book
          </button>
          <button
            v-if="updateState"
            @click="updateBook2"
            type="button"
            class="btn btn-success"
          >
            Update Book
          </button>
        </form>
      </div>
      <div class="col-8">
        <div class="row">
          <div class="col-6 mt-2" v-for="book in books" :key="book">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">Title: {{ book.title }}</h5>
                <p class="card-text">ID: {{ book.id }}</p>
                <button
                  :disabled="updateState"
                  @click="updateBook(book)"
                  class="btn btn-success mr-2"
                >
                  Update
                </button>
                <button
                  :disabled="updateState"
                  @click="deleteBook(book.id)"
                  class="btn btn-danger"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { v4 as uuidv4 } from "uuid";
export default {
  name: "Crud",
  setup() {
    const books = ref([]);
    const book = ref("");
    const bookTemp = ref({});
    const updateState = ref(false);
    function addBook() {
      books.value.push({
        id: uuidv4(),
        title: book.value,
      });

      book.value = "";
      console.log(books.value);
    }
    function deleteBook(id) {
      console.log(books.value.filter((book) => book.id != id));
      books.value = books.value.filter((book) => book.id != id);
    }
    function updateBook(bookTmp) {
      updateState.value = true;
      bookTemp.value = bookTmp;
      book.value = bookTmp.title;
    }
    function updateBook2() {
      const index = books.value.findIndex(
        (book) => book.id == bookTemp.value.id
      );
      books.value.splice(index, 1, {
        title: book.value,
        id: bookTemp.value.id,
      });
      updateState.value = false;
      book.value = "";
      console.log(index);
    }
    return {
      addBook,
      deleteBook,
      updateBook,
      updateBook2,
      updateState,
      book,
      bookTemp,
      books,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
