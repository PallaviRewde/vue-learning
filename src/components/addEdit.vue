<template>
  <div class="crud-app">
    <h1>ADD Edit Candidate Form</h1>

    <div>
      <input v-model="newCandidate.name" placeholder="Name" />
      <input v-model="newCandidate.email" placeholder="Email" />
      <button @click="addcandidate" :disabled="!newCandidate.name || !newCandidate.email">
        Add candidate
      </button>
    </div>

    <h2>Candidates List</h2>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(candidate, index) in Candidates" :key="candidate.id">
          <td>{{ candidate.name }}</td>
          <td>{{ candidate.email }}</td>
          <td>
            <button @click="editcandidate(index)">Edit</button>
            <button @click="deletecandidate(index)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <div v-if="editCandidate">
      <h3>Edit candidate</h3>
      <input v-model="editCandidate.name" placeholder="Name" />
      <input v-model="editCandidate.email" placeholder="Email" />
      <button @click="savecandidate">Save</button>
      <button @click="cancelEdit">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      Candidates: [],
      newCandidate: {
        name: '',
        email: ''
      },
      editCandidate: null,
      editingIndex: null
    };
  },
  methods: {
    addcandidate() {
      if (this.newCandidate.name && this.newCandidate.email) {
        const candidate = {
          id: this.Candidates.length + 1, // Simple ID generation
          name: this.newCandidate.name,
          email: this.newCandidate.email
        };
        this.Candidates.push(candidate);
        this.newCandidate.name = '';
        this.newCandidate.email = '';
      }
    },
    
    editcandidate(index) {
      this.editCandidate = { ...this.Candidates[index] }; // Copy data to avoid mutation
      this.editingIndex = index;
    },
    
    savecandidate() {
      if (this.editCandidate.name && this.editCandidate.email) {
        this.$set(this.Candidates, this.editingIndex, { ...this.editCandidate });
        this.cancelEdit();
      }
    },
    
    cancelEdit() {
      this.editCandidate = null;
      this.editingIndex = null;
    },
    
    deletecandidate(index) {
      this.Candidates.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.crud-app {
  font-family: Arial, sans-serif;
  padding: 20px;
}

input {
  margin-right: 10px;
  padding: 5px;
}

button {
  padding: 5px 10px;
  cursor: pointer;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

table th, table td {
  border: 1px solid #ccc;
  padding: 8px;
  text-align: left;
}

table th {
  background-color: #f4f4f4;
}

tr:hover {
  background-color: #f9f9f9;
}
</style>
