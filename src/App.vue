<template>
  <div id="app">
    <header class="app-header">
      <img src="https://storage.googleapis.com/tmg-production/1675107261-614.png" alt="Jollibee Logo" class="logo" /> 
    </header>
    <h1>Quản lý nhân viên JLB SGH</h1>

    <!-- Form thêm nhân viên -->
    <form @submit.prevent="addEmployee">
      <input
        type="text"
        v-model="newEmployee.name"
        placeholder="Tên nhân viên"
        required
      />
      <input
        type="number"
        v-model="newEmployee.age"
        placeholder="Tuổi"
        required
      />
      <select v-model="newEmployee.department" required>
        <option disabled value="">Chọn bộ phận</option>
        <option v-for="department in departments" :key="department" :value="department">
          {{ department }}
        </option>
      </select>
      <select v-model="newEmployee.position" required>
        <option disabled value="">Chọn vị trí</option>
        <option v-for="position in positions" :key="position" :value="position">
          {{ position }}
        </option>
      </select>
      <button type="submit">Thêm nhân viên</button>
    </form>

    <!-- Danh sách nhân viên -->
    <h2>Danh sách nhân viên</h2>
    <table v-if="employees.length" class="employee-table">
      <thead>
        <tr>
          <th>STT</th>
          <th>Tên</th>
          <th>Tuổi</th>
          <th>Bộ phận</th>
          <th>Vị trí</th>
          <th>Hành động</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(employee, index) in employees" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.age }}</td>
          <td>{{ employee.department }}</td>
          <td>{{ employee.position }}</td>
          <td><button @click="removeEmployee(index)">Xóa</button></td>
        </tr>
      </tbody>
    </table>
    <p v-else>Chưa có nhân viên nào.</p>

    <!-- Nút lưu danh sách nhân viên -->
    <button @click="downloadJson" v-if="employees.length">Lưu danh sách</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Dữ liệu cho nhân viên mới
      newEmployee: {
        name: "",
        age: null,
        department: "",
        position: "",
      },
      
      departments: ["dining", "counter", "kitchen"],
      positions: ["new crew", "regular crew", "crew trainer", "crew leader"],
      // Danh sách nhân viên
      employees: [],
    };
  },
  methods: {
    
    addEmployee() {
      const { name, age, department, position } = this.newEmployee;
      if (name && age && department && position) {
        this.employees.push({ name, age, department, position });
        this.newEmployee = { name: "", age: null, department: "", position: "" }; // Reset form
      }
    },
    
    removeEmployee(index) {
      this.employees.splice(index, 1);
    },
    
    downloadJson() {
      const jsonData = JSON.stringify(this.employees, null, 2);
      const blob = new Blob([jsonData], { type: "application/json" });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = "employees.json";
      link.click();
      URL.revokeObjectURL(url);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.app-header {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #414141;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.logo {
  width: 30%;
  height: auto;
  margin-right: 15px;
}

form {
  margin-bottom: 20px;
}

input,
select {
  width: 20%;
  padding: 10px;
  margin: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.employee-table {
  width: 80%;
  margin: 0 auto;
  border-collapse: collapse;
}

.employee-table th,
.employee-table td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}

.employee-table th {
  background-color: #f8f9fa;
}

.employee-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

.employee-table button {
  background-color: #dc3545;
}

.employee-table button:hover {
  background-color: #c82333;
}
</style>
