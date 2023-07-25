<template>
  <!-- Notes App Container -->
  <div class="p-2 m-2 align-self-stretch">

    <!-- Navbar -->
    <div class="navbar rounded shadow-sm p-3 mb-5 bg-white rounded">
      <h1 class="navbar-title">Notes App</h1>
      <img src="@/assets/notes.png" class="navbar-logo" alt="image note">
      <button type="button" class="btn btn-warning navbar-btn" @click="showEditor = true">ADD NOTE</button>
    </div>

    <!-- Main Notes Section -->
    <div v-if="!showEditor" class="container">
      <div class="grid grid-cols-4 gap-4 m-5">
        <div v-for="note in notes" :key="note.id" class="note-item bg-white shadow rounded p-4">
          <h3 class="note-title text-purple-700 font-bold text-xl">{{ note.title }}</h3>
          <div class="note-content text-gray-800">
            {{ note.content }}
          </div>

          <!-- Edit Mode -->
          <div v-if="editingNoteId === note.id">
            <!-- Edit Title and Content Fields -->
            <input v-model="note.title" placeholder="Title" class="form-control" />
            <textarea v-model="note.content" placeholder="Content" class="form-control"></textarea>
            <div class="edit-buttons mt-2">
              <button class="btn btn-success" @click="saveEdit(note.id)">
                <i class="fas fa-check"></i> Save
              </button>
              <button class="btn btn-secondary" @click="editingNoteId = null">
                <i class="fas fa-times"></i> Cancel
              </button>
            </div>
          </div>

          <!-- View Mode -->
          <div v-else>
            <div class="view-buttons mt-2">
              <button class="btn btn-primary" @click="editingNoteId = note.id">
                <i class="fas fa-edit"></i> Edit
              </button>
              <button class="btn btn-danger" @click="deleteNote(note.id)">
                <i class="fas fa-trash-alt"></i> Delete
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Editor Section -->
    <div v-if="showEditor" class="editor-container">
      <div class="editor-box">
        <input v-model="newNote.title" placeholder="Title" class="form-control" />
        <textarea v-model="newNote.content" placeholder="Content" class="form-control"></textarea>
        <div class="editor-buttons mt-2">
          <button class="btn btn-success" @click="saveNote">
            <i class="fas fa-save"></i> Save
          </button>
          <button class="btn btn-secondary" @click="showEditor = false">
            <i class="fas fa-times"></i> Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<!-- Script Section -->
<script>
export default {
  name: "NotesContainer",

  data() {
    return {
      // State variables
      showEditor: false,
      newNote: { title: "", content: "" },
      notes: [],
      editingNoteId: null,
    };
  },

  methods: {
    // Method to save edited note
    saveEdit(id) {
      const editedNote = this.notes.find((note) => note.id === id);
      if (editedNote) {
        this.editingNoteId = null;
      }
    },

    // Method to delete a note
    deleteNote(id) {
      const noteIndex = this.notes.findIndex((note) => note.id === id);
      if (noteIndex !== -1) {
        this.notes.splice(noteIndex, 1);
      }
    },

    // Method to save a new note
    saveNote() {
      if (this.newNote.title && this.newNote.content) {
        const newNote = { ...this.newNote, id: Date.now() };
        this.notes.push(newNote);
        this.newNote = { title: "", content: "" };
        this.showEditor = false;
      }
    },
  },
};
</script>

<!-- Styles Section -->
<style>
/* Importing styles */
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';
@import '~bootstrap/dist/css/bootstrap.min.css';

/* Navbar styles */
.navbar {
  background: linear-gradient(to right, #fb9619, #da7492);
  background-size: 100% 100%;
}

.navbar-logo {
  width: 10%;
}

.navbar-title {
  font-size: 24px;
  font-weight: bold;
}

.navbar-btn {
  font-size: 18px;
}

/* Note item styles */
.note-item {
  transition: transform 0.2s ease-in-out;
  margin-bottom: 20px;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
  background-color: #f3f4f6;
  flex: 1;
  max-width: calc(25% - 20px);
}

.note-item:hover {
  transform: scale(1.03);
}

/* Editor styles */
.editor-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 999;
}

.editor-box {
  width: 300px;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
}

.editor-box input,
.editor-box textarea {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.editor-box .editor-buttons {
  display: flex;
  justify-content: space-between;
}

.editor-box .btn {
  margin-right: 5px;
}
</style>
