<template>
  <div class="page-container">
    <!-- Page title -->
    <h1 class="page-title">Notes App</h1>
    <div v-if="!showEditor">
      <div class="notes-container">
        <!-- Loop through the notes and display each note -->
        <div v-for="note in notes" :key="note.id" class="note-item">
          <h3 class="note-title">{{ note.title }}</h3>

          <!-- Show truncated content if the note is not in edit mode -->
          <div v-if="!note.showFullContent" class="note-content">
            {{ truncateContent(note.content) }}
            <div class="see-more">
              <!-- Button to expand the content -->
              <button @click="showFullContent(note.id)">See More</button>
            </div>
          </div>

          <!-- Show full content if the note is in edit mode -->
          <div v-else>
            <div v-if="editingNoteId === note.id">
              <!-- Input fields for editing the note -->
              <input v-model="note.title" placeholder="Title" />
              <textarea v-model="note.content" placeholder="Content"></textarea>
              <div class="edit-buttons">
                <!-- Save and Cancel buttons for editing -->
                <button class="btn btn-success" @click="saveEdit(note.id)">
                  <i class="fas fa-check"></i> Save
                </button>
                <button class="btn btn-secondary" @click="cancelEdit()">
                  <i class="fas fa-times"></i> Cancel
                </button>
              </div>
            </div>
            <div v-else>
              <!-- Display the full content of the note -->
              <p>{{ note.content }}</p>
              <div class="view-buttons">
                <!-- Edit and Delete buttons for each note -->
                <button class="btn btn-primary" @click="startEdit(note.id)">
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
      
      <!-- Button to add a new note -->
      <button class="add-button" @click="showEditor = true">Add Note</button>
    </div>

    <!-- New note editor section -->
    <div v-if="showEditor" class="editor">
      <!-- Input fields for creating a new note -->
      <input v-model="newNote.title" placeholder="Title" />
      <textarea v-model="newNote.content" placeholder="Content"></textarea>
      <div class="editor-buttons">
        <!-- Save and Cancel buttons for creating a new note -->
        <button class="btn btn-success" @click="saveNote">
          <i class="fas fa-save"></i> Save
        </button>
        <button class="btn btn-secondary" @click="cancelNote">
          <i class="fas fa-times"></i> Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<!---------------------------------------------------------------->
<script>
export default {
  name: "NotesContainer",

  data() {
    return {
      // Whether to show the new note editor or not
      showEditor: false,
      
      // Object to store the data of the new note being created
      newNote: { title: "", content: "" },
      
      // Array to store the notes
      notes: [],
      
      // ID of the note currently being edited
      editingNoteId: null,
    };
  },

  methods: {
    // Function to truncate the content of a note to a specific length
    truncateContent(content) {
      const maxLength = 20;
      return content.length > maxLength ? content.substring(0, maxLength) + "..." : content;
    },

    // Function to show the full content of a note
    showFullContent(id) {
      const noteIndex = this.notes.findIndex((note) => note.id === id);
      if (noteIndex !== -1) {
        this.notes[noteIndex].showFullContent = true;
      }
    },

    // Function to enter edit mode for a specific note
    startEdit(id) {
      this.editingNoteId = id;
    },

    // Function to save the changes made to a note in edit mode
    saveEdit(id) {
      const editedNote = this.notes.find((note) => note.id === id);
      if (editedNote) {
        editedNote.showFullContent = false;
        this.editingNoteId = null;
      }
    },

    // Function to cancel the edit mode for a note
    cancelEdit() {
      this.editingNoteId = null;
    },

    // Function to delete a note
    deleteNote(id) {
      const noteIndex = this.notes.findIndex((note) => note.id === id);
      if (noteIndex !== -1) {
        this.notes.splice(noteIndex, 1);
      }
    },

    // Function to save a new note
    saveNote() {
      if (this.newNote.title && this.newNote.content) {
        const newNote = { ...this.newNote, id: Date.now(), showFullContent: false };
        this.notes.push(newNote);
        this.newNote = { title: "", content: "" };
        this.showEditor = false;
      }
    },

    // Function to cancel creating a new note
    cancelNote() {
      this.newNote = { title: "", content: "" };
      this.showEditor = false;
    },
  },
};
</script>

<!---------------------------------------------------------------->
<style>

/* Body */
body {
  background: linear-gradient(to right, #9575cd, #7986cb);
  background-size: 100% 100%; 
}

/* Main page container */
.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

/* Page title */
.page-title {
  font-size: 36px;
  margin-bottom: 2px;
  color: #4a148c; 
}

/* Add button */
.add-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 18px;
  color: white;
  background-color: #8e24aa; 
  cursor: pointer;
  transition: background-color 0.3s ease;
  position: fixed;
  bottom: 20px;
  right: 20px;
}

.add-button:hover {
  background-color: #6a1b9a; 
}

/* Notes container */
.notes-container {
  display: flow-root;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  margin-top: 20px;
}

/* Note item */
.note-item {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 20px;
  width: 300px;
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.note-item:hover {
  transform: scale(1.05);
}

/* Note title */
.note-title {
  font-size: 20px;
  font-weight: bold;
  margin: 0 0 10px;
  color: #4a148c; 
}

/* Note content */
.note-content {
  height: 100px;
  overflow: hidden;
  margin-bottom: 10px;
  color: #333; 
}

/* "See More" button in truncated notes */
.see-more {
  display: flex;
  justify-content: center;
}

.see-more button {
  border: none;
  cursor: pointer;
  color: #2962ff; 
  background: none;
  text-decoration: underline;
  font-size: 14px;
}

.see-more button:hover {
  text-decoration: none;
}

/* Editor */
.editor {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  border-radius: 8px;
  z-index: 999;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #f7f7f7;
}

/* Editor input and textarea */
.editor input, .editor textarea {
  width: 100%;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

/* Editor buttons */
.editor-buttons {
  display: flex;
  justify-content: flex-end;
}

/* Save button in editor */
.save-button {
  padding: 8px 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  margin-left: 10px;
  color: white;
  background-color: #00c853; 
  transition: background-color 0.3s ease;
}

.save-button:hover {
  background-color: #007e33; 
}

/* Cancel button in editor */
.cancel-button {
  padding: 8px 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  margin-left: 10px;
  color: black;
  background-color: #e0e0e0; 
  transition: background-color 0.3s ease;
}

.cancel-button:hover {
  background-color: #bdbdbd; 
}

</style>