<template>
  <div class="container">
    <h2>KEYWORD KEEPER</h2>
    <P>Merupakan website yang dapat membantu mencatat kata kunci yang kamu perlu catat(simpan)</P>
    <div class="form-group">
      <input type="text" v-model="inputData" placeholder="Masukkan kata kunci" @keyup.enter="addData" />
      <button @click="addData">Tambah</button>
    </div>
    <ul class="data-list">
      <li v-for="(item, index) in data" :key="index" class="data-item">
        <span v-if="index !== editIndex">{{ item }}</span>
        <form v-else @submit.prevent="saveEdit(index)">
          <input type="text" v-model="editedData" @keyup.enter="saveEdit(index)" />
          <button type="submit" class="action-button">Simpan</button>
          <button @click="cancelEdit" class="action-button">Batal</button>
        </form>
        <div>
          <button @click="editData(index)" v-if="index !== editIndex" class="action-button">Edit</button>
          <button @click="deleteData(index)" class="action-button">Hapus</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputData: "",
      data: [],
      editIndex: null,
      editedData: "",
    };
  },
  methods: {
    addData() {
      if (this.inputData.trim() !== "") {
        this.data.push(this.inputData);
        this.inputData = "";
        this.saveData();
      }
    },
    editData(index) {
      this.editIndex = index;
      this.editedData = this.data[index];
    },
    saveEdit(index) {
      if (this.editedData.trim() !== "") {
        this.data.splice(index, 1, this.editedData);
        this.editIndex = null;
        this.editedData = "";
        this.saveData();
      }
    },
    cancelEdit() {
      this.editIndex = null;
      this.editedData = "";
    },
    deleteData(index) {
      this.data.splice(index, 1);
      this.saveData();
    },
    saveData() {
      localStorage.setItem("userData", JSON.stringify(this.data));
    },
  },
  mounted() {
    const savedData = localStorage.getItem("userData");
    if (savedData) {
      this.data = JSON.parse(savedData);
    }
  },
};
</script>


<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #979797;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
}

p {
  text-align: center;
}

.form-group {
  margin-bottom: 20px;
}

input[type="text"] {
  width: calc(100% - 100px);
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.action-button {
  margin-right: 10px;
}

.data-list {
  list-style-type: none;
  padding: 0;
}

.data-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

.data-item:last-child {
  border-bottom: none;
}

form {
  display: flex;
  align-items: center;
}
</style>