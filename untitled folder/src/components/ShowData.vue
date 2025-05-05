<template>
  <div class="show-data-container">
    <header class="d-flex align-items-center justify-content-center justify-content-md-between border-bottom">
      <p class="head">Registered Donors Data</p>
    </header>
    
    <div class="table-container">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Name</th>
            <th>Blood Type</th>
            <th>Prefecture</th>
            <th>Contact</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="!donors.length">
            <td colspan="5" class="text-center">No donors registered yet</td>
          </tr>
          <tr v-for="donor in donors" :key="donor.id">
            <td>{{ `${donor.firstName} ${donor.lastName}` }}</td>
            <td>{{ donor.bloodType }}</td>
            <td>{{ donor.prefecture }}</td>
            <td>{{ donor.phone }}</td>
            <td>
              <button class="btn btn-sm btn-warning me-2" @click="editDonor(donor)">Edit</button>
              <button class="btn btn-sm btn-danger" @click="deleteDonor(donor.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Edit Modal -->
    <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editModalLabel">Edit Donor Information</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body" v-if="selectedDonor">
            <form class="row g-3">
              <div class="col-md-6">
                <label class="form-label">First Name</label>
                <input type="text" class="form-control" v-model="selectedDonor.firstName">
              </div>
              <div class="col-md-6">
                <label class="form-label">Last Name</label>
                <input type="text" class="form-control" v-model="selectedDonor.lastName">
              </div>
              <div class="col-md-6">
                <label class="form-label">Blood Type</label>
                <input type="text" class="form-control" v-model="selectedDonor.bloodType">
              </div>
              <div class="col-md-6">
                <label class="form-label">Prefecture</label>
                <input type="text" class="form-control" v-model="selectedDonor.prefecture">
              </div>
              <div class="col-md-6">
                <label class="form-label">Phone</label>
                <input type="text" class="form-control" v-model="selectedDonor.phone">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="saveChanges">Save changes</button>
          </div>
        </div>
      </div>
    </div>

    <div class="actions">
      <button class="btn btn-primary" @click="$router.push('/')">Back to Registration</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { Modal } from 'bootstrap';

const donors = ref([
  // Sample data - replace with actual data source
  {
    id: 1,
    firstName: 'John',
    lastName: 'Doe',
    bloodType: 'A+',
    prefecture: 'Tokyo',
    phone: '123-4567-7891'
  }
]);

const selectedDonor = ref(null);
let editModal = null;

onMounted(() => {
  editModal = new Modal(document.getElementById('editModal'));
});

const viewDetails = (donor) => {
  console.log('View details for:', donor);
};

const editDonor = (donor) => {
  selectedDonor.value = { ...donor }; // Create a copy of donor data
  editModal.show();
};

const saveChanges = () => {
  if (selectedDonor.value) {
    const index = donors.value.findIndex(d => d.id === selectedDonor.value.id);
    if (index !== -1) {
      donors.value[index] = { ...selectedDonor.value };
    }
    editModal.hide();
  }
};

const deleteDonor = (id) => {
  if (confirm('Are you sure you want to delete this donor?')) {
    donors.value = donors.value.filter(donor => donor.id !== id);
  }
};
</script>

<style scoped>
.show-data-container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.head {
  font-size: 32px;
  font-weight: bold;
  color: #970A30;
  text-align: center;
  width: 100%;
  margin: 20px 0;
}

.table-container {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin: 20px 0;
}

.table {
  margin-bottom: 0;
}

.actions {
  text-align: center;
  margin-top: 20px;
}

.btn-primary {
  background-color: #A00E34;
  border: none;
}

.btn-primary:hover {
  background-color: #FF4757;
}

.btn-info {
  background-color: #2196F3;
  color: white;
  border: none;
}

.btn-danger {
  background-color: #dc3545;
  border: none;
  margin-left: 5px;
}

.btn-danger:hover {
  background-color: #bb2d3b;
}

.btn-warning {
  background-color: #ffc107;
  border: none;
  color: #000;
}

.btn-warning:hover {
  background-color: #ffca2c;
  color: #000;
}
</style>